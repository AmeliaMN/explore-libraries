01\_explore-libraries\_jenny.R
================
amelia
Mon Aug 13 13:23:33 2018

``` r
# Loading packages
library(fs)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## Warning: package 'dplyr' was built under R version 3.5.1

    ## ── Conflicts ────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.5/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.5/Resources/library

Installed packages

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 127

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.5/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.5/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.5/Resources… <NA>          98

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                  60 0.472 
    ## 2 yes                 62 0.488 
    ## 3 <NA>                 5 0.0394

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   Built     n    prop
    ##   <chr> <int>   <dbl>
    ## 1 3.5.0   126 0.992  
    ## 2 3.5.1     1 0.00787

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?

## Yes! It matches with the information from the slides, 14 base packages
# and 15 recommended.

##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##  [1] "assertthat"      "backports"       "base64enc"      
    ##  [4] "BH"              "bindr"           "bindrcpp"       
    ##  [7] "bitops"          "broom"           "callr"          
    ## [10] "caTools"         "cellranger"      "cli"            
    ## [13] "clipr"           "clisymbols"      "colorspace"     
    ## [16] "crayon"          "curl"            "DBI"            
    ## [19] "dbplyr"          "desc"            "dichromat"      
    ## [22] "digest"          "dplyr"           "enc"            
    ## [25] "evaluate"        "fansi"           "fivethirtyeight"
    ## [28] "forcats"         "fs"              "ggdendro"       
    ## [31] "ggformula"       "ggplot2"         "gh"             
    ## [34] "git2r"           "glue"            "gridExtra"      
    ## [37] "gtable"          "haven"           "highr"          
    ## [40] "hms"             "htmltools"       "httr"           
    ## [43] "ini"             "jsonlite"        "knitr"          
    ## [46] "labeling"        "latticeExtra"    "lazyeval"       
    ## [49] "lubridate"       "magrittr"        "markdown"       
    ## [52] "mime"            "modelr"          "mosaic"         
    ## [55] "mosaicCore"      "mosaicData"      "munsell"        
    ## [58] "openssl"         "pillar"          "pkgconfig"      
    ## [61] "plogr"           "plyr"            "praise"         
    ## [64] "processx"        "purrr"           "R6"             
    ## [67] "RColorBrewer"    "Rcpp"            "readr"          
    ## [70] "readxl"          "rematch"         "rematch2"       
    ## [73] "reprex"          "reshape2"        "rlang"          
    ## [76] "rmarkdown"       "rprojroot"       "rstudioapi"     
    ## [79] "rvest"           "scales"          "selectr"        
    ## [82] "stringi"         "stringr"         "styler"         
    ## [85] "testthat"        "tibble"          "tidyr"          
    ## [88] "tidyselect"      "tidyverse"       "tinytex"        
    ## [91] "translations"    "usethis"         "utf8"           
    ## [94] "viridisLite"     "whisker"         "withr"          
    ## [97] "xfun"            "xml2"            "yaml"

``` r
# Not in mine! I have lots of user-installed packages in .Library

## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 FALSE     49 0.386
    ## 2 TRUE      78 0.614
