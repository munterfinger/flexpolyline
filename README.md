
# flexpolyline  <img src="man/figures/logo.png" align="right" alt="" width="120" />

<!-- badges: start -->
[![R build status](https://github.com/munterfinger/flexpolyline/workflows/R-CMD-check/badge.svg)](https://github.com/munterfinger/flexpolyline/actions)
[![pkgdown](https://github.com/munterfinger/flexpolyline/workflows/pkgdown/badge.svg)](https://github.com/munterfinger/flexpolyline/actions)
<!-- badges: end -->

The `flexpolyline` R package provides a binding to the
[C++ implementation](https://github.com/heremaps/flexible-polyline/tree/master/cpp) of the
flexible polyline encoding by [HERE](https://github.com/heremaps/flexible-polyline).
The flexible polyline encoding is a lossy compressed representation of a list of
coordinate pairs or coordinate triples. The encoding is achieved by:
(1) Reducing the decimal digits of each value;
(2) encoding only the offset from the previous point;
(3) using variable length for each coordinate delta; and
(4) using 64 URL-safe characters to display the result.
The felxible polyline encoding is a variant of the [Encoded Polyline Algorithm Format](https://developers.google.com/maps/documentation/utilities/polylinealgorithm) by Google.

**Notes:**

* Decoding gives reliable results up to a precision of 7 digits.
The tests are also limited to this range.
* The order of the coordinates (lng, lat) does not correspond to the original C ++ implementation (lat, lng).
This enables simple conversion to `sf` objects, without reordering the columns.
* The encoding is lossy, this means the encoding process could reduce the precision of your data.

## Installation

You can install the released version of flexpolyline from [CRAN](https://CRAN.R-project.org) with:

``` r
install.packages("flexpolyline")
```

Install the development version from [GitHub](https://github.com/munterfinger/flexpolyline/) with:

``` r
remotes::install_github("munterfinger/flexpolyline")
```

## Usage
### C++ binding

Encodeing and decoding in R is straight forward by using `encode()` and `decode()`:

``` r
line <- matrix(
  c(8.69821, 50.10228, 10,
    8.69567, 50.10201, 20,
    8.69150, 50.10063, 30,
    8.68752, 50.09878, 40),
  ncol = 3, byrow = TRUE
)

encode(line3d)

decode("BlBoz5xJ67i1BU1B7PUzIhaUxL7YU")
```

### Simple feature support
A common way to deal with spatial data in R is using the
[sf](https://cran.r-project.org/web/packages/sf/index.html)  package, which is
built on the concept of simple features. The functions `encode_sf()` and
`decode_sf()` are an interface which support the encoding of sf objects:

``` r
sfg <- sf::st_linestring(line, dim = "XYZ")

encode_sf(sfg)

decode_sf("BlBoz5xJ67i1BU1B7PUzIhaUxL7YU")
```

## References
* [Flexible polyline encoding by HERE](https://github.com/heremaps/flexible-polyline)
* [Encoded Polyline Algorithm Format](https://developers.google.com/maps/documentation/utilities/polylinealgorithm)
* [Simple Features for R](https://cran.r-project.org/web/packages/sf/index.html)
* Inspired by the [googlePolylines](https://github.com/SymbolixAU/googlePolylines) package

## License
* The `flexpolyline` R Package is licensed under GNU GPL v3.0 - see the [LICENSE.md](LICENSE.md) file for details.
* The C++ implementation by HERE is licensed under MIT - see the [LICENSE](inst/include/hf/LICENSE) file for details.
