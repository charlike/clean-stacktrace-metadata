# clean-stacktrace-metadata [![NPM version](https://img.shields.io/npm/v/clean-stacktrace-metadata.svg?style=flat)](https://www.npmjs.com/package/clean-stacktrace-metadata) [![mit license][license-img]][license-url] [![NPM monthly downloads](https://img.shields.io/npm/dm/clean-stacktrace-metadata.svg?style=flat)](https://npmjs.org/package/clean-stacktrace-metadata) [![npm total downloads][downloads-img]][downloads-url]

> Plugin for `clean-stacktrace` lib. Parse each line to get additional info like `filename`, `column` and `line` of the error.

[![code climate][codeclimate-img]][codeclimate-url] 
[![code style][standard-img]][standard-url] 
[![linux build][travis-img]][travis-url] 
[![windows build][appveyor-img]][appveyor-url] 
[![code coverage][coverage-img]][coverage-url] 
[![dependency status][david-img]][david-url]
[![paypal donate][paypalme-img]][paypalme-url] 

You might also be interested in [clean-stacktrace](https://github.com/tunnckocore/clean-stacktrace#readme).

## Table of Contents
- [Install](#install)
- [Usage](#usage)
- [API](#api)
  * [cleanStacktraceMetadata](#cleanstacktracemetadata)
- [Related](#related)
- [Contributing](#contributing)
- [Building docs](#building-docs)
- [Running tests](#running-tests)
- [Author](#author)
- [License](#license)

## Install
Install with [npm](https://www.npmjs.com/)

```
$ npm install clean-stacktrace-metadata --save
```

or install using [yarn](https://yarnpkg.com)

```
$ yarn add clean-stacktrace-metadata
```

## Usage
> For more use-cases see the [tests](test.js)

```js
const cleanStacktraceMetadata = require('clean-stacktrace-metadata')
```

## API

### [cleanStacktraceMetadata](index.js#L74)
> Parses each line in stack and pass `info` object to the given `plugin` function. That `plugin` function is passed with `(line, info, index)` signature, where `line` is string representing the stacktrace line, and `info` is an object with `.line`, `.column` and `.filename` properties. That's useful, you can attach them to some error object.

_Meant to be used inside `mapper` of the [clean-stacktrace][],
another useful mapper is [clean-stacktrace-relative-paths][] to show
relative paths inside stacktrace, instead of absolute paths._

**Params**

* `plugin` **{Function}**: A function passed with `(line, info, index)` signature.    
* `returns` **{Function}** `mapper`: Function that can be passed to [clean-stacktrace][]  

**Example**

```js
const metadata = require('clean-stacktrace-metadata')
const cleanStack = require('clean-stacktrace')

const err = new Error('Missing unicorn')
console.log(error.stack)
// =>
// Error: Missing unicorn
//     at quxie (/home/charlike/apps/alwa.js:8:10)
//     at module.exports (/home/charlike/apps/foo.js:6:3)
//     at Object.<anonymous> (/home/charlike/apps/dush.js:45:3)
//     at Module._compile (module.js:409:26)
//     at Object.Module._extensions..js (module.js:416:10)
//     at Module.load (module.js:343:32)
//     at Function.Module._load (module.js:300:12)
//     at Function.Module.runMain (module.js:441:10)
//     at startup (node.js:139:18)

const mapper = metadata((line, info, index) => {
  if (index === 1) {
    err.line = info.line
    err.column = info.column
    err.filename = info.filename
    err.place = info.place
  }

  return line
})

const stack = cleanStack(error.stack, mapper)
console.log(stack)
// =>
// Error: Missing unicorn
//     at quxie (/home/charlike/apps/alwa.js:8:10)
//     at module.exports (/home/charlike/apps/foo.js:6:3)
//     at Object.<anonymous> (/home/charlike/apps/dush.js:45:3)

console.log(err.place) // => 'quxie'
console.log(err.line) // => 8
console.log(err.column) // => 10
console.log(err.filename) // => '/home/charlike/apps/alwa.js'
```

## Related
- [always-done](https://www.npmjs.com/package/always-done): Handle completion and errors with elegance! Support for streams, callbacks, promises, child processes, async/await and sync functions. A drop-in replacement… [more](https://github.com/hybridables/always-done#readme) | [homepage](https://github.com/hybridables/always-done#readme "Handle completion and errors with elegance! Support for streams, callbacks, promises, child processes, async/await and sync functions. A drop-in replacement for [async-done][] - pass 100% of its tests plus more")
- [assert-kindof](https://www.npmjs.com/package/assert-kindof): Check native type of value and throw AssertionError if not okey. Clean stack traces. Simplicity. Built on [is-kindof][]. | [homepage](https://github.com/tunnckocore/assert-kindof#readme "Check native type of value and throw AssertionError if not okey. Clean stack traces. Simplicity. Built on [is-kindof][].")
- [clean-stack](https://www.npmjs.com/package/clean-stack): Clean up error stack traces | [homepage](https://github.com/sindresorhus/clean-stack#readme "Clean up error stack traces")
- [clean-stacktrace-relative-paths](https://www.npmjs.com/package/clean-stacktrace-relative-paths): Meant to be used with [clean-stacktrace][] as mapper function. Makes absolute paths inside stack traces to relative paths. | [homepage](https://github.com/tunnckocore/clean-stacktrace-relative-paths#readme "Meant to be used with [clean-stacktrace][] as mapper function. Makes absolute paths inside stack traces to relative paths.")
- [clean-stacktrace](https://www.npmjs.com/package/clean-stacktrace): Clean up error stack traces from node internals | [homepage](https://github.com/tunnckocore/clean-stacktrace#readme "Clean up error stack traces from node internals")
- [is-kindof](https://www.npmjs.com/package/is-kindof): Check type of given javascript value. Support promises, generators, streams, and native types. Built on [kind-of][] lib. | [homepage](https://github.com/tunnckocore/is-kindof#readme "Check type of given javascript value. Support promises, generators, streams, and native types. Built on [kind-of][] lib.")
- [minibase](https://www.npmjs.com/package/minibase): Minimalist alternative for Base. Build complex APIs with small units called plugins. Works well with most of the already existing… [more](https://github.com/node-minibase/minibase#readme) | [homepage](https://github.com/node-minibase/minibase#readme "Minimalist alternative for Base. Build complex APIs with small units called plugins. Works well with most of the already existing [base][] plugins.")
- [stacktrace-metadata](https://www.npmjs.com/package/stacktrace-metadata): Modify given `err` object to be more useful. Adds `line`, `column` and `filename` properties. | [homepage](https://github.com/tunnckocore/stacktrace-metadata#readme "Modify given `err` object to be more useful. Adds `line`, `column` and `filename` properties.")
- [try-catch-core](https://www.npmjs.com/package/try-catch-core): Low-level package to handle completion and errors of sync or asynchronous functions, using [once][] and [dezalgo][] libs. Useful for and… [more](https://github.com/hybridables/try-catch-core#readme) | [homepage](https://github.com/hybridables/try-catch-core#readme "Low-level package to handle completion and errors of sync or asynchronous functions, using [once][] and [dezalgo][] libs. Useful for and used in higher-level libs such as [always-done][] to handle completion of anything.")

## Contributing
Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/tunnckoCore/clean-stacktrace-metadata/issues/new).  
Please read the [contributing guidelines](CONTRIBUTING.md) for advice on opening issues, pull requests, and coding standards.  
If you need some help and can spent some cash, feel free to [contact me at CodeMentor.io](https://www.codementor.io/tunnckocore?utm_source=github&utm_medium=button&utm_term=tunnckocore&utm_campaign=github) too.

**In short:** If you want to contribute to that project, please follow these things

1. Please DO NOT edit [README.md](README.md), [CHANGELOG.md](CHANGELOG.md) and [.verb.md](.verb.md) files. See ["Building docs"](#building-docs) section.
2. Ensure anything is okey by installing the dependencies and run the tests. See ["Running tests"](#running-tests) section.
3. Always use `npm run commit` to commit changes instead of `git commit`, because it is interactive and user-friendly. It uses [commitizen][] behind the scenes, which follows Conventional Changelog idealogy.
4. Do NOT bump the version in package.json. For that we use `npm run release`, which is [standard-version][] and follows Conventional Changelog idealogy.

Thanks a lot! :)

## Building docs
Documentation and that readme is generated using [verb-generate-readme][], which is a [verb][] generator, so you need to install both of them and then run `verb` command like that

```
$ npm install verbose/verb#dev verb-generate-readme --global && verb
```

_Please don't edit the README directly. Any changes to the readme must be made in [.verb.md](.verb.md)._

## Running tests
Clone repository and run the following in that cloned directory

```
$ npm install && npm test
```

## Author
**Charlike Mike Reagent**

+ [github/tunnckoCore](https://github.com/tunnckoCore)
+ [twitter/tunnckoCore](https://twitter.com/tunnckoCore)
+ [codementor/tunnckoCore](https://codementor.io/tunnckoCore)

## License
Copyright © 2017, [Charlike Mike Reagent](https://i.am.charlike.online). Released under the [MIT License](LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.4.3, on March 13, 2017._  
_Project scaffolded using [charlike][] cli._

[always-done]: https://github.com/hybridables/always-done
[async-done]: https://github.com/gulpjs/async-done
[base]: https://github.com/node-base/base
[charlike]: https://github.com/tunnckocore/charlike
[clean-stack]: https://github.com/sindresorhus/clean-stack
[clean-stacktrace]: https://github.com/tunnckocore/clean-stacktrace
[commitizen]: https://github.com/commitizen/cz-cli
[dezalgo]: https://github.com/npm/dezalgo
[is-kindof]: https://github.com/tunnckocore/is-kindof
[kind-of]: https://github.com/jonschlinkert/kind-of
[once]: https://github.com/isaacs/once
[standard-version]: https://github.com/conventional-changelog/standard-version
[verb-generate-readme]: https://github.com/verbose/verb-generate-readme
[verb]: https://github.com/verbose/verb

[license-url]: https://www.npmjs.com/package/clean-stacktrace-metadata
[license-img]: https://img.shields.io/npm/l/clean-stacktrace-metadata.svg

[downloads-url]: https://www.npmjs.com/package/clean-stacktrace-metadata
[downloads-img]: https://img.shields.io/npm/dt/clean-stacktrace-metadata.svg

[codeclimate-url]: https://codeclimate.com/github/tunnckoCore/clean-stacktrace-metadata
[codeclimate-img]: https://img.shields.io/codeclimate/github/tunnckoCore/clean-stacktrace-metadata.svg

[travis-url]: https://travis-ci.org/tunnckoCore/clean-stacktrace-metadata
[travis-img]: https://img.shields.io/travis/tunnckoCore/clean-stacktrace-metadata/master.svg?label=linux

[appveyor-url]: https://ci.appveyor.com/project/tunnckoCore/clean-stacktrace-metadata
[appveyor-img]: https://img.shields.io/appveyor/ci/tunnckoCore/clean-stacktrace-metadata/master.svg?label=windows

[coverage-url]: https://codecov.io/gh/tunnckoCore/clean-stacktrace-metadata
[coverage-img]: https://img.shields.io/codecov/c/github/tunnckoCore/clean-stacktrace-metadata/master.svg

[david-url]: https://david-dm.org/tunnckoCore/clean-stacktrace-metadata
[david-img]: https://img.shields.io/david/tunnckoCore/clean-stacktrace-metadata.svg

[standard-url]: https://github.com/feross/standard
[standard-img]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg

[paypalme-url]: https://www.paypal.me/tunnckoCore
[paypalme-img]: https://img.shields.io/badge/paypal-donate-brightgreen.svg

[clean-stacktrace-relative-paths]: https://github.com/tunnckocore/clean-stacktrace-relative-paths