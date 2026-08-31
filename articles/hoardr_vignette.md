# Introduction to the hoardr package

## hoardr introduction

`hoardr` is a package for managing cached files.

The benefit of using `hoardr` vs. raw `rapddirs` is that `hoardr`
exposes an easy to use `R6` class that has variables and functions
within it, so you don’t have to import function foo or bar, etc. Just a
single object.

You can easily wrap `hoardr` with user facing functions in your own
package to manage cached files.

If you find any bugs or have any feature requests get in touch at
<https://github.com/ropensci/hoardr>.

### Install

Stable from CRAN

``` r

install.packages("hoardr")
```

Dev version

``` r

devtools::install_github(c("ropensci/hoardr"))
```

``` r

library("hoardr")
```

### initialize client

``` r

(x <- hoardr::hoard())
#> <hoard> 
#>   path: 
#>   cache path:
```

### set cache path

``` r

x$cache_path_set("foobar", type = 'tempdir')
#> [1] "/tmp/RtmpEpj1Z7/R/foobar"
```

### make the directory if doesn’t exist

``` r

x$mkdir()
```

### put a file in the cache

``` r

cat("hello world", file = file.path(x$cache_path_get(), "foo.txt"))
```

### list the files

``` r

x$list()
#> [1] "/tmp/RtmpEpj1Z7/R/foobar/foo.txt"
```

### details

``` r

x$details()
#> <cached files>
#>   directory: /tmp/RtmpEpj1Z7/R/foobar
#> 
#>   file: /foo.txt
#>   size: 0 mb
```

### delete by file name

``` r

x$delete("foo.txt")
x$list()
#> character(0)
```
