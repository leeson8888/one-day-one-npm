HTTP status utility for node.

## Installation

This is a [Node.js](https://nodejs.org/en/) module available through the
[npm registry](https://www.npmjs.com/). Installation is done using the
[`npm install` command](https://docs.npmjs.com/getting-started/installing-npm-packages-locally):

```sh
$ npm install statuses
```

## API

<!-- eslint-disable no-unused-vars -->

```js
var status = require('statuses')
```

### var code = status(Integer || String)

If `Integer` or `String` is a valid HTTP code or status message, then the
appropriate `code` will be returned. Otherwise, an error will be thrown.

<!-- eslint-disable no-undef -->

```js
status(403) // => 403
status('403') // => 403
status('forbidden') // => 403
status('Forbidden') // => 403
status(306) // throws, as it's not supported by node.js
```

### status.codes

Returns an array of all the status codes as `Integer`s.

### var msg = status[code]

Map of `code` to `status message`. `undefined` for invalid `code`s.

<!-- eslint-disable no-undef, no-unused-expressions -->

```js
status[404] // => 'Not Found'
```

### var code = status[msg]

Map of `status message` to `code`. `msg` can either be title-cased or
lower-cased. `undefined` for invalid `status message`s.

<!-- eslint-disable no-undef, no-unused-expressions -->

```js
status['not found'] // => 404
status['Not Found'] // => 404
```

### status.redirect[code]

Returns `true` if a status code is a valid redirect status.

<!-- eslint-disable no-undef, no-unused-expressions -->

```js
status.redirect[200] // => undefined
status.redirect[301] // => true
```

### status.empty[code]

Returns `true` if a status code expects an empty body.

<!-- eslint-disable no-undef, no-unused-expressions -->

```js
status.empty[200] // => undefined
status.empty[204] // => true
status.empty[304] // => true
```

### status.retry[code]

Returns `true` if you should retry the rest.

<!-- eslint-disable no-undef, no-unused-expressions -->

```js
status.retry[501] // => undefined
status.retry[503] // => true
```

## Adding Status Codes

The status codes are primarily sourced from
https://www.iana.org/assignments/http-status-codes/http-status-codes-1.csv.
Additionally, custom codes are added from
https://en.wikipedia.org/wiki/List_of_HTTP_status_codes. These are added
manually in the `lib/*.json` files. If you would like to add a status code,
add it to the appropriate JSON file.

To rebuild `codes.json`, run the following:

```bash
# update src/iana.json
npm run fetch
# build codes.json
npm run build
```