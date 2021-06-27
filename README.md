<!--

@license Apache-2.0

Copyright (c) 2018 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

# incrmme

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] [![dependencies][dependencies-image]][dependencies-url]

> Compute a moving [mean error][mean-absolute-error] (ME) incrementally.

<section class="intro">

For a window of size `W`, the [mean error][mean-absolute-error] is defined as

<!-- <equation class="equation" label="eq:mean_error" align="center" raw="\operatorname{ME} = \frac{1}{W} \sum_{i=0}^{W-1} (y_i - x_i)" alt="Equation for the mean error."> -->

<div class="equation" align="center" data-raw-text="\operatorname{ME} = \frac{1}{W} \sum_{i=0}^{W-1} (y_i - x_i)" data-equation="eq:mean_error">
    <img src="https://cdn.rawgit.com/stdlib-js/stdlib/634ac3689760e2f57fd51085f387d8dc5bb3b927/lib/node_modules/@stdlib/stats/incr/mme/docs/img/equation_mean_error.svg" alt="Equation for the mean error.">
    <br>
</div>

<!-- </equation> -->

</section>

<!-- /.intro -->

<section class="installation">

## Installation

```bash
npm install @stdlib/stats-incr-mme
```

</section>

<section class="usage">

## Usage

```javascript
var incrmme = require( '@stdlib/stats-incr-mme' );
```

#### incrmme( window )

Returns an accumulator `function` which incrementally computes a moving [mean error][mean-absolute-error]. The `window` parameter defines the number of values over which to compute the moving [mean error][mean-absolute-error].

```javascript
var accumulator = incrmme( 3 );
```

#### accumulator( \[x, y] )

If provided input values `x` and `y`, the accumulator function returns an updated [mean error][mean-absolute-error]. If not provided input values `x` and `y`, the accumulator function returns the current [mean error][mean-absolute-error].

```javascript
var accumulator = incrmme( 3 );

var m = accumulator();
// returns null

// Fill the window...
m = accumulator( 2.0, 3.0 ); // [(2.0,3.0)]
// returns 1.0

m = accumulator( -1.0, 4.0 ); // [(2.0,3.0), (-1.0,4.0)]
// returns 3.0

m = accumulator( 3.0, 9.0 ); // [(2.0,3.0), (-1.0,4.0), (3.0,9.0)]
// returns 4.0

// Window begins sliding...
m = accumulator( -7.0, 3.0 ); // [(-1.0,4.0), (3.0,9.0), (-7.0,3.0)]
// returns 7.0

m = accumulator( -5.0, -3.0 ); // [(3.0,9.0), (-7.0,3.0), (-5.0,-3.0)]
// returns 6.0

m = accumulator();
// returns 6.0
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Input values are **not** type checked. If provided `NaN` or a value which, when used in computations, results in `NaN`, the accumulated value is `NaN` for **at least** `W-1` future invocations. If non-numeric inputs are possible, you are advised to type check and handle accordingly **before** passing the value to the accumulator function.
-   As `W` (x,y) pairs are needed to fill the window buffer, the first `W-1` returned values are calculated from smaller sample sizes. Until the window is full, each returned value is calculated from all provided values.
-   Be careful when interpreting the [mean error][mean-absolute-error] as errors can cancel. This stated, that errors can cancel makes the [mean error][mean-absolute-error] suitable for measuring the bias in forecasts.
-   **Warning**: the [mean error][mean-absolute-error] is scale-dependent and, thus, the measure should **not** be used to make comparisons between datasets having different scales.

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var randu = require( '@stdlib/random-base-randu' );
var incrmme = require( '@stdlib/stats-incr-mme' );

var accumulator;
var v1;
var v2;
var i;

// Initialize an accumulator:
accumulator = incrmme( 5 );

// For each simulated datum, update the moving mean error...
for ( i = 0; i < 100; i++ ) {
    v1 = ( randu()*100.0 ) - 50.0;
    v2 = ( randu()*100.0 ) - 50.0;
    accumulator( v1, v2 );
}
console.log( accumulator() );
```

</section>

<!-- /.examples -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2021. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-incr-mme.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-incr-mme

[test-image]: https://github.com/stdlib-js/stats-incr-mme/actions/workflows/test.yml/badge.svg
[test-url]: https://github.com/stdlib-js/stats-incr-mme/actions/workflows/test.yml

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-incr-mme/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-incr-mme?branch=main

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-incr-mme.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-incr-mme/main

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://gitter.im/stdlib-js/stdlib/

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-incr-mme/main/LICENSE

[mean-absolute-error]: https://en.wikipedia.org/wiki/Mean_absolute_error

</section>

<!-- /.links -->
