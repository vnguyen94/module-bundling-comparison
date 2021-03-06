# JavaScript Module Bundling Comparison

## Overview
- compares the popular module bundlers of today (**October 2016**)
  using the latest versions (includes betas)
- see the underlying structural differences and similarities between each bundler
- `import { get } from 'lodash'` imports every lodash function along with it, while
  `import get from 'lodash/get'` imports just the one function. This is the reason why
  both default and direct importing are tested.

## The program
1. `src/index.js` imports fn1()...8() using ES6 modules
2. executes fn1()...8()
3. all functions call `lodash.get()`
4. fn9() is imported and called from fn8()
5. fn10() is imported but not called from fn8() (tree-shakeable)
6. `console.log`s fn1()...9()

## Stats (ordered by size desc)
### Default import
```sh
bundle-jspm.js 554K
bundle-browserify.js 525K
bundle-browserify-rollup.js 523K
bundle-webpack.js 473K
bundle-rollup.js NONE (cannot import lodash directly)
```
### Default import + UglifyJS
```sh
bundle-webpack.min.js 145K
bundle-browserify.min.js 141K
bundle-jspm.min.js 141K
bundle-browserify-rollup.min.js 139K
bundle-rollup.js NONE (cannot import lodash directly)
```
### Direct import
```sh
bundle-jspm.js 45K
bundle-webpack.js 43K
bundle-browserify.js 37K
bundle-browserify-rollup.js 30K
bundle-rollup.js 29K
```
### Direct import + UglifyJS
```sh
bundle-jspm.min.js 24K
bundle-webpack.min.js 22K
bundle-browserify.min.js 19K
bundle-browserify-rollup.min.js 13K
bundle-rollup.min.js 12K
```

## Notes
- Rollup struggles with non-ES6 modules; `import { get } from 'lodash'` does not load `get` into the
  bundle, but `import { get } from 'lodash-es'` does.
- As the tests show, tree-shaking does not work on unused methods from objects that *are* used. This is
  why when lodash is imported by default, Rollup cannot remove all of the unused methods.

## To run
- `npm run all` to concurrently create bundles and then concurrently create minified bundles
- `npm run all:min`
- `ls -lah -S -R dist/* | awk '{print $9, $5}'` to read the bundle filesizes, ordered by size desc

##### Basically Nolan Lawson's [Rollup Comparison](https://github.com/nolanlawson/rollup-comparison), but with a lodash dependency and an unused export.
