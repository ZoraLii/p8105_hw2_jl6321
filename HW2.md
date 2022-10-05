p8105_hw2_jl6321.Rmd
================

## Problem 2

``` r
data_mr_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Mr. Trash Wheel", range = "A2:N549") %>%
  janitor::clean_names()
data_mr_trash$sports_balls = as.integer(round(data_mr_trash$sports_balls)) 
```

``` r
data_prof_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Professor Trash Wheel", range = "A2:M96") %>%
  janitor::clean_names()
```

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

``` r
View(data_pols_month)
```
