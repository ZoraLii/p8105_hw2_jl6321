p8105_hw2_jl6321.Rmd
================

## Problem 2

``` r
library(readxl)
data_mr_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Mr. Trash Wheel", range = "A2:N549") 
data_mr_trash$`Sports Balls` = as.integer(round(data_mr_trash$`Sports Balls`)) 
```

``` r
library(readxl)
data_prof_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Professor Trash Wheel", range = "A2:M96") 
```

``` r
data_mr_trash$Trash_wheel = "Mr"
data_prof_trash$Trash_wheel = "Prof"
data_mr_trash$Year = as.double(data_mr_trash$Year)
data_joined_trash = full_join(data_mr_trash, data_prof_trash)
```

``` r
View(data_joined_trash)
```
