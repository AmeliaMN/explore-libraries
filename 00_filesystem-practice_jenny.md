00\_filesystem-practice\_jenny.R
================
amelia
Mon Aug 13 13:11:57 2018

``` r
## First attempt: Just get it to work ----

list.files("~/Desktop/day1_s1_explore-libraries")
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
list.files("~/Desktop/day1_s1_explore-libraries", pattern = "\\.R$")
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
list.files(
  "~/Desktop/day1_s1_explore-libraries",
  pattern = "\\.R$",
  full.names = TRUE
)
```

    ## [1] "/Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_comfy.R"  
    ## [2] "/Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_jenny.R"  
    ## [3] "/Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_spartan.R"

``` r
from_files <- list.files(
  "~/Desktop/day1_s1_explore-libraries",
  pattern = "\\.R$",
  full.names = TRUE
)

(to_files <- basename(from_files))
```

    ## [1] "01_explore-libraries_comfy.R"   "01_explore-libraries_jenny.R"  
    ## [3] "01_explore-libraries_spartan.R"

``` r
file.copy(from_files, to_files)
```

    ## [1]  TRUE FALSE  TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "01_explore-libraries_comfy.R"         
    ## [6] "01_explore-libraries_jenny.R"         
    ## [7] "01_explore-libraries_spartan.R"       
    ## [8] "explore-libraries.Rproj"              
    ## [9] "README.md"

``` r
## Clean it out, so we can refine ----
file.remove(to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "explore-libraries.Rproj"              
    ## [6] "README.md"

``` r
## Copy again, tighter code ----
from_dir <- file.path("~", "Desktop", "day1_s1_explore-libraries")
from_files <- list.files(from_dir, pattern = "\\.R$", full.names = TRUE)
to_files <- basename(from_files)
file.copy(from_files, to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "01_explore-libraries_comfy.R"         
    ## [6] "01_explore-libraries_jenny.R"         
    ## [7] "01_explore-libraries_spartan.R"       
    ## [8] "explore-libraries.Rproj"              
    ## [9] "README.md"

``` r
## Clean it out, so we can use fs ----
file.remove(to_files)
```

    ## [1] TRUE TRUE TRUE

``` r
list.files()
```

    ## [1] "00_filesystem-practice_jenny.html"    
    ## [2] "00_filesystem-practice_jenny.R"       
    ## [3] "00_filesystem-practice_jenny.spin.R"  
    ## [4] "00_filesystem-practice_jenny.spin.Rmd"
    ## [5] "explore-libraries.Rproj"              
    ## [6] "README.md"

``` r
## Copy again, using fs ----
library(fs)
(from_dir <- path_home("Desktop", "day1_s1_explore-libraries"))
```

    ## /Users/amelia/Desktop/day1_s1_explore-libraries

``` r
(from_files <- dir_ls(from_dir, glob = "*.R"))
```

    ## /Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_comfy.R
    ## /Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_jenny.R
    ## /Users/amelia/Desktop/day1_s1_explore-libraries/01_explore-libraries_spartan.R

``` r
(to_files <- path_file(from_files))
```

    ## 01_explore-libraries_comfy.R   01_explore-libraries_jenny.R   
    ## 01_explore-libraries_spartan.R

``` r
(out <- file_copy(from_files, to_files))
```

    ## 01_explore-libraries_comfy.R   01_explore-libraries_jenny.R   
    ## 01_explore-libraries_spartan.R

``` r
dir_ls()
```

    ## 00_filesystem-practice_jenny.R
    ## 00_filesystem-practice_jenny.html
    ## 00_filesystem-practice_jenny.spin.R
    ## 00_filesystem-practice_jenny.spin.Rmd
    ## 01_explore-libraries_comfy.R
    ## 01_explore-libraries_jenny.R
    ## 01_explore-libraries_spartan.R
    ## README.md
    ## explore-libraries.Rproj

``` r
dir_info()
```

    ##                                    path type    size permissions
    ## 1        00_filesystem-practice_jenny.R file   1.72K   rw-r--r--
    ## 2     00_filesystem-practice_jenny.html file 729.35K   rw-r--r--
    ## 3   00_filesystem-practice_jenny.spin.R file   1.72K   rw-r--r--
    ## 4 00_filesystem-practice_jenny.spin.Rmd file   1.82K   rw-r--r--
    ## 5          01_explore-libraries_comfy.R file   1.12K   rw-r--r--
    ## 6          01_explore-libraries_jenny.R file   1.54K   rw-r--r--
    ## 7        01_explore-libraries_spartan.R file     850   rw-r--r--
    ## 8                             README.md file      75   rw-r--r--
    ## 9               explore-libraries.Rproj file     204   rw-r--r--
    ##     modification_time   user group device_id hard_links special_device_id
    ## 1 2018-08-13 13:11:56 amelia staff  16777220          1                 0
    ## 2 2018-08-13 13:07:54 amelia staff  16777220          1                 0
    ## 3 2018-08-13 13:11:57 amelia staff  16777220          1                 0
    ## 4 2018-08-13 13:11:57 amelia staff  16777220          1                 0
    ## 5 2018-08-12 14:24:30 amelia staff  16777220          1                 0
    ## 6 2018-08-12 14:24:30 amelia staff  16777220          1                 0
    ## 7 2018-08-12 14:24:30 amelia staff  16777220          1                 0
    ## 8 2018-08-13 11:54:12 amelia staff  16777220          1                 0
    ## 9 2018-08-13 11:54:13 amelia staff  16777220          1                 0
    ##      inode block_size blocks flags generation         access_time
    ## 1 60215742       4096      8     0          0 2018-08-13 13:11:57
    ## 2 60216618       4096   1464     0          0 2018-08-13 13:09:04
    ## 3 60216743       4096      8     0          0 2018-08-13 13:11:57
    ## 4 60216744       4096      8     0          0 2018-08-13 13:11:57
    ## 5 60216751       4096      8     0          0 2018-08-13 13:11:57
    ## 6 60216752       4096      8     0          0 2018-08-13 13:11:57
    ## 7 60216753       4096      8     0          0 2018-08-13 13:11:57
    ## 8 60215565       4096      8     0          0 2018-08-13 11:55:03
    ## 9 60215566       4096      8     0          0 2018-08-13 11:59:16
    ##           change_time          birth_time
    ## 1 2018-08-13 13:11:56 2018-08-12 14:42:55
    ## 2 2018-08-13 13:07:54 2018-08-13 13:07:54
    ## 3 2018-08-13 13:11:57 2018-08-13 13:11:57
    ## 4 2018-08-13 13:11:57 2018-08-13 13:11:57
    ## 5 2018-08-13 13:11:57 2018-08-12 14:24:30
    ## 6 2018-08-13 13:11:57 2018-08-12 14:24:30
    ## 7 2018-08-13 13:11:57 2018-08-12 14:24:30
    ## 8 2018-08-13 11:54:12 2018-08-13 11:54:12
    ## 9 2018-08-13 11:54:13 2018-08-13 11:54:12

``` r
## Sidebar: Why does Jenny name files this way? ----
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
ft <- tibble(files = dir_ls(glob = "*.R"))
ft
```

    ## # A tibble: 5 x 1
    ##   files                              
    ##   <fs::path>                         
    ## 1 00_filesystem-practice_jenny.R     
    ## 2 00_filesystem-practice_jenny.spin.R
    ## 3 01_explore-libraries_comfy.R       
    ## 4 01_explore-libraries_jenny.R       
    ## 5 01_explore-libraries_spartan.R

``` r
ft %>%
  filter(str_detect(files, "explore"))
```

    ## # A tibble: 3 x 1
    ##   files                         
    ##   <fs::path>                    
    ## 1 01_explore-libraries_comfy.R  
    ## 2 01_explore-libraries_jenny.R  
    ## 3 01_explore-libraries_spartan.R

``` r
ft %>%
  mutate(files = path_ext_remove(files)) %>%
  separate(files, into = c("i", "topic", "flavor"), sep = "_")
```

    ## # A tibble: 5 x 3
    ##   i     topic               flavor    
    ##   <chr> <chr>               <chr>     
    ## 1 00    filesystem-practice jenny     
    ## 2 00    filesystem-practice jenny.spin
    ## 3 01    explore-libraries   comfy     
    ## 4 01    explore-libraries   jenny     
    ## 5 01    explore-libraries   spartan

``` r
dirs <- dir_ls(path_home("Desktop"), type = "directory")
(dt <- tibble(dirs = path_file(dirs)))
```

    ## # A tibble: 3 x 1
    ##   dirs                     
    ##   <fs::path>               
    ## 1 day1_s1_explore-libraries
    ## 2 day1_s2_copy-files       
    ## 3 explore-libraries

``` r
dt %>%
  separate(dirs, into = c("day", "session", "topic"), sep = "_")
```

    ## Warning: Expected 3 pieces. Missing pieces filled with `NA` in 1 rows [3].

    ## # A tibble: 3 x 3
    ##   day               session topic            
    ##   <chr>             <chr>   <chr>            
    ## 1 day1              s1      explore-libraries
    ## 2 day1              s2      copy-files       
    ## 3 explore-libraries <NA>    <NA>

``` r
## Principled use of delimiters --> meta-data can be recovered from filename

## Clean it out, so we reset for workshop ----
file_delete(to_files)
dir_ls()
```

    ## 00_filesystem-practice_jenny.R
    ## 00_filesystem-practice_jenny.html
    ## 00_filesystem-practice_jenny.spin.R
    ## 00_filesystem-practice_jenny.spin.Rmd
    ## README.md
    ## explore-libraries.Rproj
