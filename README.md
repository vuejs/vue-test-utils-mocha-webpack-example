# vue-test-utils-mocha-example

> Example project using mocha-webpack and vue-test-utils

This is based on the `vue-cli` `webpack-simple` template. Test-specific changes include:

### Additional Dependencies

- `vue-test-utils`
- `mocha` & `mocha-webpack`
- `jsdom` & `jsdom-global` (for setting up DOM environment in tests)
- `webpack-node-externals` (for excluding NPM deps from test bundle)
- `expect` (for assertions)
  - This is the package used internally by Jest, so [usage is the same](http://facebook.github.io/jest/docs/en/expect.html#content). You can also use [chai](http://chaijs.com/) + [sinon](http://sinonjs.org/).

### Additional Configuration

#### `package.json`

Added `test` script:

``` js
{
  // ...
  "scripts": {
    // ...
    "test": "cross-env NODE_ENV=test mocha-webpack --webpack-config webpack.config.js --require test/setup.js test/**/*.spec.js"
  }
}
```

#### `webpack.config.js`

Added test-specific configs:

``` js
if (process.env.NODE_ENV === 'test') {
  // exclude NPM deps from test bundle
  module.exports.externals = [require('webpack-node-externals')()]
  // use inline source map so that it works with mocha-webpack
  module.exports.devtool = 'inline-cheap-module-source-map'
}
```

#### `test/setup.js`

Global setup for tests. This is run first with `mocha-webpack`'s `--require` flag.

``` js
// setup JSDOM
require('jsdom-global')()

// make expect available globally
global.expect = require('expect')
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# run unit tests
npm test
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
