[![npm version](https://img.shields.io/npm/v/volleyball.svg?maxAge=3600)](https://www.npmjs.com/package/volleyball)
[![Codeship](https://img.shields.io/codeship/6f38c760-44b0-0134-bda6-02154be91b77.svg)](https://codeship.com/projects/168493)
[![Greenkeeper.io is keeping this repo's dependencies up to date](https://img.shields.io/badge/greenkeeper.io-monitoring-brightgreen.svg?maxAge=3600)](https://greenkeeper.io/)

# 🏐 Volleyball

### Small Express HTTP logger for debugging asynchronous requests and responses

```js
const app = require('express')()
const volleyball = require('volleyball')

app.use(volleyball)
```

![Screenshot](https://cloud.githubusercontent.com/assets/7230206/17677905/ce1c59f0-6302-11e6-9bdb-b43c4d831152.jpg)

Volleyball is a minimal Connect-style middleware function which logs incoming requests and outgoing responses as separate events. It optionally supports the [`debug`](https://github.com/visionmedia/debug#readme) module.

## Motivation

Logging HTTP cycles can be approached in several ways, each with its own drawbacks:

* Log only upon **request**. Drawback: we cannot log the corresponding response, which happens later (if at all).
* Log only upon **response**, attaching the request. Drawback A: if the server never sends a response, e.g. due to a bug, the request will not be logged either. Drawback B: two temporally distinct events are conflated, misleadingly.
* Log both upon **request** and **response**. Drawback: it is not necessarily clear which responses are for which requests.

Volleyball takes the last approach, and assigns randomly-generated ids to label request-response pairs. It is designed for student project development, teaching beginning programmers how HTTP servers and asynchronicity work. It may also be useful as a low-configuration debug tool.

## Usage

### `volleyball`

The `volleyball` module can be used directly as demonstrated in the first example. The module defaults to using `process.stdout` for output.

### `volleyball.custom(config)`

A customized logging middleware can be generated by calling `custom` with a configuration object:

```js
const logger = volleyball.custom({ debug: true }) // default namespace 'http'
// const logger = volleyball.custom({ debug: 'custom-namespace' })
// const logger = volleyball.custom({ debug: debugInstance })

app.use(logger)
```

The `debug` property logs using the [`debug`](https://github.com/visionmedia/debug#readme) module. It supports the following values:

value | result
----|----
`true` | uses a new debug instance with a default namespace of `'http'`
string | uses a new debug instance with a custom namespace
function | uses any function, such as a pre-generated debug instance. Note that the function will be called with colorized strings.

## Related and Alternatives

For more powerful, configurable, and compatible logging needs, check out:

* [debug](https://github.com/visionmedia/debug#readme)
* [morgan](https://github.com/expressjs/morgan#readme)
* [morgan-debug](https://github.com/ChiperSoft/morgan-debug#readme)
* [winston](https://github.com/winstonjs/winston#readme)
* [express-winston](https://github.com/bithavoc/express-winston#readme)
* [node-bunyan](https://github.com/trentm/node-bunyan/#readme)
