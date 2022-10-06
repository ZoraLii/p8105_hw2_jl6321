p8105_hw2_jl6321.Rmd
================

## Problem 1

``` r
trans_ent = 
  read_csv(
    "NYC_Transit_Subway_Entrance_And_Exit_Data.csv",
    col_types = cols(Route8 = "c", Route9 = "c", Route10 = "c", Route11 = "c")) %>% 
  janitor::clean_names() %>% 
  select(
    line, station_name, station_latitude, station_longitude, 
    starts_with("route"), entry, exit_only, vending, entrance_type, 
    ada) %>% 
  mutate(entry = ifelse(entry == "YES", TRUE, FALSE))
trans_ent %>% 
  select(station_name, line) %>% 
  distinct
trans_ent %>% 
  filter(ada == TRUE) %>% 
  select(station_name, line) %>% 
  distinct
trans_ent %>% 
  filter(vending == "NO") %>% 
  pull(entry) %>% 
  mean
trans_ent %>% 
  pivot_longer(
    route1:route11,
    names_to = "route_num",
    values_to = "route") %>% 
  filter(route == "A") %>% 
  select(station_name, line) %>% 
  distinct

trans_ent %>% 
  pivot_longer(
    route1:route11,
    names_to = "route_num",
    values_to = "route") %>% 
  filter(route == "A", ada == TRUE) %>% 
  select(station_name, line) %>% 
  distinct
```

## Problem 2

Read and clean the Mr. Trash Wheel sheet.

``` r
data_mr_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Mr. Trash Wheel", range = "A2:N549") %>%
  janitor::clean_names()
data_mr_trash$sports_balls = as.integer(round(data_mr_trash$sports_balls)) 
```

Apply similar procedure on Professor Wheel sheet.

``` r
data_prof_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Professor Trash Wheel", range = "A2:M96") %>%
  janitor::clean_names()
```

Combine the two datasets.

``` r
data_mr_trash$trash_wheel = "mr"
data_prof_trash$trash_wheel = "prof"
data_mr_trash$year = as.double(data_mr_trash$year)
data_joined_trash = full_join(data_mr_trash, data_prof_trash)
```

    ## Joining, by = c("dumpster", "month", "year", "date", "weight_tons",
    ## "volume_cubic_yards", "plastic_bottles", "polystyrene", "cigarette_butts",
    ## "glass_bottles", "grocery_bags", "chip_bags", "homes_powered", "trash_wheel")

In the resulting dataset, there are 641 observations in total. Some
examples of key variables are dumpster, year, weight_tons, and
sports_balls. The total weight of trash collected by Professor Trash
Wheel is 190.12 tons. The total number of sports balls collected by
Mr. Trash Wheel in 2020 is 856.

## Problem 3

Clean the pols-month dataset.

``` r
data_pols_month = read.csv("pols-month.csv") %>%
  separate(mon, into = c("year", "month", "day"), sep = '-') %>% 
  mutate(president = recode(prez_gop, `1` = "gop", `0` = "dem"))
```

    ## Warning: Unreplaced values treated as NA as `.x` is not compatible.
    ## Please specify replacements exhaustively or supply `.default`.

``` r
data_pols_month$month = month.abb[as.numeric(data_pols_month$month)] 
data_pols_month = subset(data_pols_month, select = -c(prez_dem, prez_gop, day))
```

Clean the data in snp.csv.

``` r
data_snp = read.csv("snp.csv") %>%
  separate(date, into = c("month", "day", "year"), sep = '/') 
data_snp$year = as.numeric(data_snp$year)
data_snp = mutate(data_snp, year = case_when(year < 22 ~ 2000 + year, year > 22 ~ 1900 + year)) %>% 
  relocate(year) 
data_snp$month = month.abb[as.numeric(data_snp$month)] 
data_snp = subset(data_snp, select = -c(day))
```

Tidy the unemployment data.

``` r
data_unemployment = read.csv("unemployment.csv") %>% 
  pivot_longer(
    Jan:Dec, 
    names_to = "month", 
    values_to = "unemployment"
  ) %>% 
  janitor::clean_names()
```

Joining all three datasets.

``` r
data_pols_month$year = as.numeric(data_pols_month$year)
two_dfs_merged = full_join(data_snp, data_pols_month)
```

    ## Joining, by = c("year", "month")

``` r
all_dfs_merged = full_join(data_unemployment, two_dfs_merged)
```

    ## Joining, by = c("year", "month")

The dataset data_pols_month contains year, month, gov_gop, sen_gop,
rep_gop, gov_dem, sen_dem, rep_dem, president. The dataset data_snp
contains year, month, close. The dataset data_unemployment contains
year, month, unemployment. The resulting dataset is all_dfs_merged,
which contains all variables in the previous three datasets. It has 11
variables and 828 rows. The year variable ranges from 1947 to 2015. Some
key variables include year, month, unemployment, close, and president.
