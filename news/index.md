# Changelog

## hoardr 0.5.5

CRAN release: 2025-01-18

#### BUG FIXES

- Some tests began to fail on MS Windows, probably because of mixed
  slash directions. Slash directions are now normalised
  ([\#26](https://github.com/ropensci/hoardr/issues/26)).

## hoardr 0.5.4

CRAN release: 2024-01-23

#### BUG FIXES

- [`testthat::test_check()`](https://testthat.r-lib.org/reference/test_package.html)
  failed when full path for cache dir was
  [`tempdir()`](https://rdrr.io/r/base/tempfile.html). Replaced
  [`tempdir()`](https://rdrr.io/r/base/tempfile.html) with a full path
  that works ([\#23](https://github.com/ropensci/hoardr/issues/23)).

## hoardr 0.5.3

CRAN release: 2023-01-26

- New maintainer ([\#17](https://github.com/ropensci/hoardr/issues/17)).

## hoardr 0.5.2

CRAN release: 2018-12-01

#### BUG FIXES

- Important fix: `HoardClient`, called by
  [`hoardr()`](https://docs.ropensci.org/hoardr/reference/hoardr-package.md)
  function, was storing the cache path in an environment inside the R6
  class. If multiple instances of `HoardClient` exist in the same R
  session, the cache path for any one then affects all others. Fixed by
  storing as a private variable int he R6 class instead of in an
  environment ([\#14](https://github.com/ropensci/hoardr/issues/14)).

## hoardr 0.5.0

CRAN release: 2018-10-13

#### NEW FEATURES

- Gains new method on the `HoardClient` object to check if one or more
  files exist, returning a data.frame
  ([\#10](https://github.com/ropensci/hoardr/issues/10)).
- `cache_path_set()` method on `HoardClient` gains new parameter
  `full_path` to make the base cache path directly with a full path
  rather than using the three other parameters (`path`, `type`, and
  `prefix`) ([\#12](https://github.com/ropensci/hoardr/issues/12)).

## hoardr 0.2.0

CRAN release: 2017-05-10

#### CHANGES

- Compliance with CRAN policies about writing to users disk
  ([\#6](https://github.com/ropensci/hoardr/issues/6)).

#### MINOR IMPROVEMENTS

- Improved documentation
  ([\#7](https://github.com/ropensci/hoardr/issues/7)).

#### BUG FIXES

- Change `key()` and `keys()` to use `file=TRUE`
  ([\#8](https://github.com/ropensci/hoardr/issues/8)).
- Fix R6 import warning
  ([\#5](https://github.com/ropensci/hoardr/issues/5)).

## hoardr 0.1.0

CRAN release: 2017-04-21

#### NEW FEATURES

- released to CRAN
