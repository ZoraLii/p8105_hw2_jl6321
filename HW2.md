p8105_hw2_jl6321.Rmd
================

## Problem 2

``` r
library(readxl)
data_mr_trash = read_excel("Trash Wheel Collection Data.xlsx", sheet = "Mr. Trash Wheel", range = "A2:N549") 
data_mr_trash$`Sports Balls` = round(data_mr_trash$`Sports Balls`, 0)
```

``` r
View(data_mr_trash)
```
