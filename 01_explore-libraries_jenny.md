01\_explore-libraries\_jenny.R
================
amelia
Mon Aug 13 13:24:38 2018

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

``` r
# Session info
sessionInfo()
```

    ## R version 3.5.0 (2018-04-23)
    ## Platform: x86_64-apple-darwin15.6.0 (64-bit)
    ## Running under: OS X El Capitan 10.11.6
    ## 
    ## Matrix products: default
    ## BLAS: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRblas.0.dylib
    ## LAPACK: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRlapack.dylib
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] bindrcpp_0.2.2  forcats_0.3.0   stringr_1.3.1   dplyr_0.7.6    
    ##  [5] purrr_0.2.5     readr_1.1.1     tidyr_0.8.1     tibble_1.4.2   
    ##  [9] ggplot2_3.0.0   tidyverse_1.2.1 fs_1.2.5       
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_0.12.18     cellranger_1.1.0 pillar_1.3.0     compiler_3.5.0  
    ##  [5] plyr_1.8.4       bindr_0.1.1      tools_3.5.0      digest_0.6.15   
    ##  [9] lubridate_1.7.4  jsonlite_1.5     evaluate_0.11    nlme_3.1-137    
    ## [13] gtable_0.2.0     lattice_0.20-35  pkgconfig_2.0.1  rlang_0.2.1     
    ## [17] cli_1.0.0        rstudioapi_0.7   yaml_2.2.0       haven_1.1.2     
    ## [21] withr_2.1.2      xml2_1.2.0       httr_1.3.1       knitr_1.20      
    ## [25] hms_0.4.2        rprojroot_1.3-2  grid_3.5.0       tidyselect_0.2.4
    ## [29] glue_1.3.0       R6_2.2.2         fansi_0.2.3      readxl_1.1.0    
    ## [33] rmarkdown_1.10   modelr_0.1.2     magrittr_1.5     backports_1.1.2 
    ## [37] scales_0.5.0     htmltools_0.3.6  rvest_0.3.2      assertthat_0.2.0
    ## [41] colorspace_1.3-2 utf8_1.1.4       stringi_1.2.4    lazyeval_0.2.1  
    ## [45] munsell_0.5.0    broom_0.5.0      crayon_1.3.4
