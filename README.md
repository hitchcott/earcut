## Earcut

The fastest and smallest JavaScript polygon triangulation library. 2.5KB gzipped.

[![Build Status](https://travis-ci.org/mapbox/earcut.svg?branch=master)](https://travis-ci.org/mapbox/earcut)
[![Coverage Status](https://coveralls.io/repos/mapbox/earcut/badge.svg?branch=master)](https://coveralls.io/r/mapbox/earcut?branch=master)

The library implements a modified ear slicing algorithm,
optimized by [z-order curve](http://en.wikipedia.org/wiki/Z-order_curve) hashing
and extended to handle holes, twisted polygons, degeneracies and self-intersections
in a way that doesn't _guarantee_ correctness of triangulation,
but attempts to always produce acceptable results for practical data like geographical shapes.

It's based on ideas from
[FIST: Fast Industrial-Strength Triangulation of Polygons](http://www.cosy.sbg.ac.at/~held/projects/triang/triang.html) by Martin Held
and [Triangulation by Ear Clipping](http://www.geometrictools.com/Documentation/TriangulationByEarClipping.pdf) by David Eberly.

#### Why another triangulation library?

The aim of this project is to create a JS triangulation library
that is **fast enough for real-time triangulation in the browser**,
sacrificing triangulation quality for raw speed and simplicity,
while being robust enough to handle most practical datasets without crashing or producing garbage.
Some benchmarks:

(ops/sec)         | pts  | earcut    | libtess  | poly2tri | pnltri
------------------| ---- | --------- | -------- | -------- | ---------
OSM building      | 15   | _580,351_ | _27,832_ | _28,151_ | _216,352_
dude shape        | 94   | _29,848_  | _6,194_  | _3,575_  | _13,027_
holed dude shape  | 104  | _18,688_  | _5,428_  | _3,378_  | _2,264_
complex OSM water | 2523 | _445_     | _63.72_  | failure  | failure
huge OSM water    | 5667 | _80.09_   | _23.73_  | failure  | failure

The original use case it was created for is [Mapbox GL](https://www.mapbox.com/mapbox-gl), WebGL-based interactive maps.

If you want to get correct triangulation even on very bad data with lots of self-intersections
and earcut is not precise enough, take a look at [libtess.js](https://github.com/brendankenny/libtess.js).

#### Usage

```js
var triangles = earcut([[[10,0],[0,50],[60,60],[70,10]]]);
// [[0,50],[10,0],[70,10], [70,10],[60,60],[0,50]]
```

Input should be an array of rings, where the first is outer ring and others are holes;
each ring is an array of points, where each point is of the `[x, y]` form.

Each group of three points in the resulting array forms a triangle.

#### Install

NPM and Browserify:

```bash
npm install earcut
```

Browser builds:

```bash
npm install
npm run build-dev # builds dist/earcut.dev.js, a dev version with a source map
npm run build-min # builds dist/earcut.min.js, a minified production build
```

Running tests:

```bash
npm test
```

![](https://cloud.githubusercontent.com/assets/25395/5778431/e8ec0c10-9da3-11e4-8d4e-a2ced6a7d2b7.png)

#### Changelog

##### 1.2.2 (Jan 27, 2015)

- Significantly improved performance for polygons with self-intersections
  (e.g. big OSM water polygons are now handled 2-3x faster)

##### 1.2.1 (Jan 26, 2015)

- Significantly improved performance on polygons with high number of vertices
  by using z-order curve hashing for vertice lookup.
- Slightly improved overall performance with better point filtering.

##### 1.1.0 (Jan 21, 2015)

- Improved performance on polygons with holes by switching from Held to Eberly hole elimination algorithm
- More robustness fixes and tests

##### 1.0.1 &mdash; 1.0.6 (Jan 20, 2015)

- Various robustness improvements and fixes.

##### 1.0.0 (Jan 18, 2015)

- Initial release.
