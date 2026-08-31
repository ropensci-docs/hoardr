# hoardr class

hoardr class

## Arguments

- path:

  (character) a path to cache files in. required

- type:

  (character) type of cache. One of "user_cache_dir" (default),
  "user_log_dir", "user_data_dir", "user_config_dir", "site_data_dir",
  "site_config_dir". Can also pass in any function that gives a path to
  a directory, e.g.,
  [`tempdir()`](https://rdrr.io/r/base/tempfile.html). required.

## Details

For the purposes of caching, you'll likely want to stick with
`user_cache_dir`, but you can change the type of cache with the `type`
parameter.

`hoard` is just a tiny wrapper around `HoardClient$new()`, which isn't
itself exported, but you can use it if you want via `:::`

**Methods**

- `cache_path_get()`:

  Get the cache path **return**: (character) path to the cache directory

- `cache_path_set(path = NULL, type = "user_cache_dir", prefix = "R", full_path = NULL)`:

  Set the cache path. By default, we set cache path to
  `file.path(user_cache_dir, prefix, path)`. Note that this does not
  actually make the directory, but just sets the path to it.

  - path (character) the path to be appended to the cache path set by
    `type`

  - type (character) the type of cache, see
    [`rappdirs::rappdirs()`](https://rappdirs.r-lib.org/reference/rappdirs-package.html).

  - prefix (character) prefix to the `path` value. Default: "R"

  - full_path (character) instead of using `path`, `type`, and `prefix`
    just set the full path with this parameter

  **return**: (character) path to the cache directory just set

- [`list()`](https://rdrr.io/r/base/list.html):

  List files in the directory (full file paths) **return**: (character)
  vector of file paths for files in the cache

- `mkdir()`:

  Make the directory if doesn't exist already **return**: `TRUE`,
  invisibly

- `delete(files, force = TRUE)`:

  Delete files by name

  - files (character) vector/list of file paths

  - force (logical) force deletion? Default: `TRUE`

  **return**: nothing

- `delete_all(force = TRUE)`:

  Delete all files

  - force (logical) force deletion? Default: `FALSE`

  **return**: nothing

- `details(files = NULL)`:

  Get file details

  - files (character) vector/list of file paths

  **return**: objects of class `cache_info`, each with brief summary
  info including file path and file size

- `keys(algo = "md5")`:

  Get a hash for all files. Note that these keys may not be unique if
  the files are identical, leading to identical hashes **return**:
  (character) hashes for the files

- `key(x, algo = "md5")`:

  Get a hash for a single file. Note that these keys may not be unique
  if the files are identical, leading to identical hashes

  - x (character) path to a file

  - algo (character) the algorithm to be used, passed on to
    [`digest::digest()`](https://eddelbuettel.github.io/digest/man/digest.html),
    choices: md5 (default), sha1, crc32, sha256, sha512, xxhash32,
    xxhash64 and murmur32.

  **return**: (character) hash for the file

- `files()`:

  Get all files as HoardFile objects **return**: (character) paths to
  the files

- `compress()`:

  Compress files into a zip file - leaving only the zip file **return**:
  (character) path to the cache directory

- `uncompress()`:

  Uncompress all files and remove zip file **return**: (character) path
  to the cache directory

- `exists(files)`:

  Check if files exist

  - files: (character) one or more files, paths are optional

  **return**: (data.frame) with two columns:

  - files: (character) file path

  - exists: (boolean) does it exist or not

## Examples

``` r
(x <- hoard())
#> <hoard> 
#>   path: 
#>   cache path: 
x$cache_path_set(path = "foobar", type = 'tempdir')
#> [1] "/tmp/RtmpWddieL/R/foobar"
x
#> <hoard> 
#>   path: foobar
#>   cache path: /tmp/RtmpWddieL/R/foobar
x$path
#> [1] "foobar"
x$cache_path_get()
#> [1] "/tmp/RtmpWddieL/R/foobar"

# Or you can set the full path directly with `full_path`
mydir <- file.path(tempdir(), "foobar")
x$cache_path_set(full_path = mydir)
#> [1] "/tmp/RtmpWddieL/foobar"
x
#> <hoard> 
#>   path: foobar
#>   cache path: /tmp/RtmpWddieL/foobar
x$path
#> [1] "foobar"
x$cache_path_get()
#> [1] "/tmp/RtmpWddieL/foobar"

# make the directory if doesn't exist already
x$mkdir()

# list files in dir
x$list()
#> character(0)
cat(1:10000L, file = file.path(x$cache_path_get(), "foo.txt"))
x$list()
#> [1] "/tmp/RtmpWddieL/foobar/foo.txt"

# add more files
cat(letters, file = file.path(x$cache_path_get(), "foo2.txt"))
cat(LETTERS, file = file.path(x$cache_path_get(), "foo3.txt"))

# see if files exist
x$exists("foo.txt") # exists
#>                            files exists
#> 1 /tmp/RtmpWddieL/foobar/foo.txt   TRUE
x$exists(c("foo.txt", "foo3.txt")) # both exist
#>                             files exists
#> 1  /tmp/RtmpWddieL/foobar/foo.txt   TRUE
#> 2 /tmp/RtmpWddieL/foobar/foo3.txt   TRUE
x$exists(c("foo.txt", "foo3.txt", "stuff.txt")) # one doesn't exist
#>                              files exists
#> 1   /tmp/RtmpWddieL/foobar/foo.txt   TRUE
#> 2  /tmp/RtmpWddieL/foobar/foo3.txt   TRUE
#> 3 /tmp/RtmpWddieL/foobar/stuff.txt  FALSE

# cache details
x$details()
#> <cached files>
#>   directory: /tmp/RtmpWddieL/foobar
#> 
#>   file: /foo.txt
#>   size: 0.049 mb
#> 
#>   file: /foo2.txt
#>   size: 0 mb
#> 
#>   file: /foo3.txt
#>   size: 0 mb
#> 

# delete files by name - we prepend the base path for you
x$delete("foo.txt")
x$list()
#> [1] "/tmp/RtmpWddieL/foobar/foo2.txt" "/tmp/RtmpWddieL/foobar/foo3.txt"
x$details()
#> <cached files>
#>   directory: /tmp/RtmpWddieL/foobar
#> 
#>   file: /foo2.txt
#>   size: 0 mb
#> 
#>   file: /foo3.txt
#>   size: 0 mb
#> 

# delete all files
cat("one\ntwo\nthree", file = file.path(x$cache_path_get(), "foo.txt"))
cat("asdfasdf asd fasdf", file = file.path(x$cache_path_get(), "bar.txt"))
x$delete_all()
x$list()
#> character(0)

# make/get a key for a file
cat(1:10000L, file = file.path(x$cache_path_get(), "foo.txt"))
x$keys()
#> [1] "fe28308b4b9c4b40998cf4d5927a15ed"
x$key(x$list()[1])
#> [1] "fe28308b4b9c4b40998cf4d5927a15ed"

# as files
Map(function(z) z$exists(), x$files())
#> [[1]]
#> [1] TRUE
#> 

# compress and uncompress
x$compress()
#> compressed!
x$uncompress()
#> uncompressed!

# reset cache path
x$cache_path_set(path = "stuffthings", type = "tempdir")
#> [1] "/tmp/RtmpWddieL/R/stuffthings"
x
#> <hoard> 
#>   path: stuffthings
#>   cache path: /tmp/RtmpWddieL/R/stuffthings
x$cache_path_get()
#> [1] "/tmp/RtmpWddieL/R/stuffthings"
x$list()
#> character(0)

# cleanup
unlink(x$cache_path_get())
```
