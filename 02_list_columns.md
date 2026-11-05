List Columns
================

``` r
library(tidyverse)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(p8105.datasets)

source("source/mean_and_sd.R")
source("source/nsduh_process.R")

set.seed(1236)
```

## List columns …

``` r
l = list(
  vec_numeric = 5:17,
  mat         = matrix(1:8, nrow = 2, ncol = 4, byrow = T),
  vec_logical = c(TRUE),
  summary     = summary(rnorm(1000)))

l
```

    ## $vec_numeric
    ##  [1]  5  6  7  8  9 10 11 12 13 14 15 16 17
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    5    6    7    8
    ## 
    ## $vec_logical
    ## [1] TRUE
    ## 
    ## $summary
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -2.866342 -0.637111 -0.002336  0.003323  0.658024  3.265081

``` r
l[[1]]
```

    ##  [1]  5  6  7  8  9 10 11 12 13 14 15 16 17

``` r
l[[2]]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    5    6    7    8

``` r
l[["vec_logical"]]
```

    ## [1] TRUE

``` r
l$summary_table
```

    ## NULL

## `for` loop

``` r
list_norm = list(
  a = rnorm(20, 3, 4),
  b = rnorm(20, -15, 20),
  c = rnorm(20, 1000, 2000),
  d = rnorm(20, 0, 1)
)

list_norm
```

    ## $a
    ##  [1]  0.9159114  3.9351002  0.4542432 -2.2204479  6.8932190  5.1244742
    ##  [7]  3.0167339  1.2722880  2.2738587  1.3975934  0.2412513  8.7954751
    ## [13]  5.4102001  5.6806398  1.4349576  7.7910554  1.3815394  3.9662007
    ## [19]  5.5049290 -4.4350801
    ## 
    ## $b
    ##  [1] -39.827313 -18.304847 -38.049149 -11.966250 -14.882002 -43.469283
    ##  [7]  23.901561   9.711728  -2.235079 -12.917442  15.247918  22.767993
    ## [13] -11.855880  -3.384032 -28.502148 -12.016026  -1.957898 -55.728431
    ## [19]  -1.693433  20.784310
    ## 
    ## $c
    ##  [1]  -611.58801  1779.31897   949.53337 -1661.78878  1766.12591  2002.40181
    ##  [7]   292.05042  1414.89904  1016.85755  3041.46445  1781.00218    69.45088
    ## [13]  3577.47221  1092.69677   790.68923  2555.96902  -977.09748  1688.74195
    ## [19]    35.15380    14.75012
    ## 
    ## $d
    ##  [1]  1.95387846 -0.05245400 -0.46954531  0.07402405 -1.58979325 -1.30688268
    ##  [7] -1.10632794 -0.56723500 -0.41408053  0.92099945  1.44702754  1.99236409
    ## [13] -0.55596915 -0.28278326  0.08300015  1.09988202 -1.89214254  0.10662779
    ## [19]  0.16717212 -0.72687919

Can i get mean and sd for each sample?

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  3.32

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -10.2  22.7

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 1031. 1329.

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0560  1.09

``` r
# Better?
output = vector("list", length = 4)

for(i in 1:4) {

  output[[i]] = mean_and_sd(list_norm[[i]])
}

output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  3.32
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -10.2  22.7
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 1031. 1329.
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0560  1.09

## `map` instead

This is Better!

``` r
output = map(list_norm, mean_and_sd)
output = map(list_norm, median)
```

``` r
output = map_df(list_norm, mean_and_sd)
output = map_dbl(list_norm, median)  
# c(median(list_norm[[1]], median(list_norm[[2]])))
```
