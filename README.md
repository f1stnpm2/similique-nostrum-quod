# @f1stnpm2/similique-nostrum-quod

Tiny, isomorphic convenience wrapper around the [Fetch
API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) aiming to
reduce boilerplate, especially when sending and receiving JSON.

| Build            | Unminified | Minified | Gzipped |
| ---------------- | ---------- | -------- | ------- |
| ESM bundle       | 3.36 kB    | 1.54 kB  | 798 B   |
| UMD bundle       | 3.87 kB    | 1.71 kB  | 873 B   |
| UMD bundle (ES5) | 4.12 kB    | 1.86 kB  | 894 B   |

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Quick Start](#quick-start)
  - [NPM](#npm)
  - [CDN (Unpkg)](#cdn-unpkg)
- [Why?](#why)
  - [Less Boilerplate](#less-boilerplate)
  - [Isomorphism & Full ESM Support](#isomorphism--full-esm-support)
- [Package Exports](#package-exports)
  - ['@f1stnpm2/similique-nostrum-quod'](#@f1stnpm2/similique-nostrum-quod)
  - ['@f1stnpm2/similique-nostrum-quod/error'](#@f1stnpm2/similique-nostrum-quoderror)
  - ['@f1stnpm2/similique-nostrum-quod/factory'](#@f1stnpm2/similique-nostrum-quodfactory)
  - [CDN (Unpkg)](#cdn-unpkg-1)
- [API](#api)
  - [Usage](#usage)
    - [@f1stnpm2/similique-nostrum-quod(url, options)](#@f1stnpm2/similique-nostrum-quodurl-options)
    - [@f1stnpm2/similique-nostrum-quod\[method\](url, body?, options?)](#@f1stnpm2/similique-nostrum-quod%5Cmethod%5Curl-body-options)
  - [Options](#options)
    - [`baseUrl`](#baseurl)
    - [`body`](#body)
    - [`response`](#response)
    - [`searchParams`](#searchparams)
  - [.extend(defaults, api) || .extend(fnc)](#extenddefaults-api--extendfnc)
  - [Factory](#factory)
- [Credits](#credits)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Quick Start

### NPM
```sh
$ npm install @f1stnpm2/similique-nostrum-quod
```

```js
import @f1stnpm2/similique-nostrum-quod, { FetchError } from '@f1stnpm2/similique-nostrum-quod'
// or
const @f1stnpm2/similique-nostrum-quod = require('@f1stnpm2/similique-nostrum-quod')

@f1stnpm2/similique-nostrum-quod('/get-stuff').then(json => console.log(json))
@f1stnpm2/similique-nostrum-quod('/get-stuff', { response: 'blob', headers: { accept: 'image/png' } }).then(blob => console.log(blob))
@f1stnpm2/similique-nostrum-quod('/get-stuff', 'blob').then(blob => console.log(blob))

@f1stnpm2/similique-nostrum-quod.post('/do-stuff', { stuff: 'to be done' }).then(json => console.log(json))

@f1stnpm2/similique-nostrum-quod.put('/do-stuff', { stuff: 'to be done' }, { redirect: false, response: 'text' }).then(text => console.log(text))
@f1stnpm2/similique-nostrum-quod.put('/do-stuff', { stuff: 'to be done' }, 'text').then(text => console.log(text))
```

### CDN (Unpkg)

```html
<script src="https://unpkg.com/@f1stnpm2/similique-nostrum-quod"></script>
```

```js
import @f1stnpm2/similique-nostrum-quod, { FetchError } from 'https://unpkg.com/@f1stnpm2/similique-nostrum-quod/dist/@f1stnpm2/similique-nostrum-quod.esm.min.js'
```

## Why?

### Less Boilerplate

Even though the [Fetch
API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) is
significantly nicer to work with than XHR, it still quickly becomes verbose to
do simple tasks. To create a relatively simple `POST` request using JSON, `fetch`
requires something like:

```js
fetch('/api/peeps', {
  method: 'POST',
  headers: {
    'content-type': 'application/json',
    accept: 'application/json',
  },
  credentials: 'same-origin',
  body: JSON.stringify({
    name: 'James Brown',
  }),
}).then((res) => {
  if (res.ok) {
    return res.json()
  }

  const err = new Error(response.statusText)

  err.response = response
  err.status = response.status

  throw err
}).then((person) => {
  console.log(person)
}).catch(...)
```

With `@f1stnpm2/similique-nostrum-quod` this simply becomes:

```js
@f1stnpm2/similique-nostrum-quod.post('/api/peeps', { name: 'James Brown' }).then((person) => {
  console.log(person)
}).catch(...)
```

### Isomorphism & Full ESM Support

`@f1stnpm2/similique-nostrum-quod` uses [conditional
exports](https://nodejs.org/api/packages.html#packages_conditions_definitions)
to load Node or browser (and Deno) compatible ESM or CJS files. `main` and
`module` are also set in `package.json` for compatibility with legacy Node and
legacy build systems. In Node, the main entry point uses
[node-fetch](https://github.com/node-fetch/node-fetch), which needs to be
installed manually. In all other environments, the main entry point uses
[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API),
[Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers),
[URL](https://developer.mozilla.org/en-US/docs/Web/API/URL),
[URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) and
[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData) defined in the
global scope.

Internally, the ESM & CJS entry points use the same CJS files to prevent the
dual package hazard. See [the Node
documentation](https://nodejs.org/api/packages.html#packages_dual_commonjs_es_module_packages)
for more information.

## Package Exports

### '@f1stnpm2/similique-nostrum-quod'

The main entry point returns a `@f1stnpm2/similique-nostrum-quod` instance. When using `import`, a named
export `FetchError` is also exposed.

```js
import @f1stnpm2/similique-nostrum-quod, { FetchError } from '@f1stnpm2/similique-nostrum-quod'
// or
const @f1stnpm2/similique-nostrum-quod = require('@f1stnpm2/similique-nostrum-quod')
```

The browser/Deno version is created using (see [factory](#factory) for details):

```js
export default factory({
  credentials: 'same-origin',
  response: 'json',
  fetch,
  Headers,
})
```

The Node version is created using:

```js
import fetch from 'node-fetch'

export default factory({
  credentials: 'same-origin',
  response: 'json',
  fetch,
  Headers: fetch.Headers,
})
```

The main entry exposes most TypeScript types:

```ts
import { Options, Rek } from '@f1stnpm2/similique-nostrum-quod'
```

### '@f1stnpm2/similique-nostrum-quod/error'

Exports the `FetchError`. Both `import` and `require` will load
`./dist/error.cjs` in all environments.

```js
// import
import FetchError from '@f1stnpm2/similique-nostrum-quod/error'
// require
const FetchError = require('@f1stnpm2/similique-nostrum-quod/error')
// legacy
const FetchError = require('@f1stnpm2/similique-nostrum-quod/dist/error.cjs')
```

### '@f1stnpm2/similique-nostrum-quod/factory'

Exports the [factory](#factory) function that creates `@f1stnpm2/similique-nostrum-quod` instances with new
defaults. Both `import` and `require` will load `./dist/factory.cjs` in
all environments.

```js
import factory from '@f1stnpm2/similique-nostrum-quod/factory'
// require
const factory = require('@f1stnpm2/similique-nostrum-quod/factory')
// legacy
const factory = require('@f1stnpm2/similique-nostrum-quod/dist/factory.cjs')
```

The factory entry also exposes TypeScript types:

```ts
import { Options, Rek } from '@f1stnpm2/similique-nostrum-quod'
```

### CDN (Unpkg)

On top of CJS and ESM files, bundles to be consumed through
[unpkg.com](https://unpkg.com) are built into `./dist/`. The `unpkg` field
in `package.json` points at the minified UMD build. This means:

```html
<script src="https://unpkg.com/@f1stnpm2/similique-nostrum-quod"></script>
```

is the same as

```html
<script src="https://unpkg.com/@f1stnpm2/similique-nostrum-quod/dist/@f1stnpm2/similique-nostrum-quod.umd.min.js"></script>
```

To use the ES5 compatible UMD bundle:

```html
<script src="https://unpkg.com/@f1stnpm2/similique-nostrum-quod/dist/@f1stnpm2/similique-nostrum-quod.umd.es5.min.js"></script>
```

The ESM bundle can be imported from a JS file:

```js
import @f1stnpm2/similique-nostrum-quod, { FetchError } from 'https://unpkg.com/@f1stnpm2/similique-nostrum-quod/dist/@f1stnpm2/similique-nostrum-quod.esm.min.js'
```

The following bundles are available in the dist folder:

+ `./dist/@f1stnpm2/similique-nostrum-quod.esm.js` - ESM bundle
+ `./dist/@f1stnpm2/similique-nostrum-quod.esm.min.js` - Minified ESM bundle
+ `./dist/@f1stnpm2/similique-nostrum-quod.umd.js` - UMD bundle
+ `./dist/@f1stnpm2/similique-nostrum-quod.umd.min.js` - Minified UMD bundle
+ `./dist/@f1stnpm2/similique-nostrum-quod.umd.es5.js` - ES5 compatible UMD bundle
+ `./dist/@f1stnpm2/similique-nostrum-quod.umd.es5.min.js` - Minified ES5 compatible UMD bundle

## API

### Usage

#### @f1stnpm2/similique-nostrum-quod(url, options?)

Makes a request with `fetch` and returns a parsed response body or the
[Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) (depending
on the [response](#response) option). If `res.ok` is not true, an error is thrown.
See [options](#options) below for differences to native `fetch` options.

```js
@f1stnpm2/similique-nostrum-quod('/url').then(json => { ... })
@f1stnpm2/similique-nostrum-quod('/url', { response: 'text' }).then(text => { ... })
@f1stnpm2/similique-nostrum-quod('/url', { body: { plain: 'object' }, method: 'post' }).then(json => { ... })
```

#### @f1stnpm2/similique-nostrum-quod\[method\](url, body?, options?)

`@f1stnpm2/similique-nostrum-quod` has convenience methods for all relevant HTTP request methods. They
set the correct `option.method` and have an extra `body` argument
when the request can send bodies (the `body` argument overwrites `options.body`).

- __@f1stnpm2/similique-nostrum-quod.delete(url, options?)__
- __@f1stnpm2/similique-nostrum-quod.get(url, options?)__
- __@f1stnpm2/similique-nostrum-quod.head(url, options?)__
- __@f1stnpm2/similique-nostrum-quod.patch(url, body?, options?)__
- __@f1stnpm2/similique-nostrum-quod.post(url, body?, options?)__
- __@f1stnpm2/similique-nostrum-quod.put(url, body?, options?)__

```js
@f1stnpm2/similique-nostrum-quod.delete('/api/peeps/1337')
// is the same as
@f1stnpm2/similique-nostrum-quod('/api/peeps/1337', { method: 'DELETE' })

@f1stnpm2/similique-nostrum-quod.post('/api/peeps/14', { name: 'Max Powers' })
// is the same as
@f1stnpm2/similique-nostrum-quod('/api/peeps/14', { method: 'POST', body: { name: 'Max Powers' } })
```

### Options

`@f1stnpm2/similique-nostrum-quod` supports three arguments on top of the default fetch
options:
[baseUrl](#baseurl), [response](#response) and [searchParams](#searchParams). It also handles
`body` differently to native `fetch()`.

Options passed to `@f1stnpm2/similique-nostrum-quod` will be merged with the `defaults` defined in the [factory](#factory).

If a string is passed as option argument, a new object is created with
`response` set to that string.

```js
@f1stnpm2/similique-nostrum-quod('/', 'text')
// is the same
@f1stnpm2/similique-nostrum-quod('/', { response: 'text' })
```

#### `baseUrl`

A URL that relative paths will be resolved against.

Setting this in `defaults` is very useful for SSR and similar.

#### `body`

Depending on the type of body passed, it could be converted to a JSON string
and the `content-type` header could be removed or set

+ __FormData || URLSearchParams__: `body` will not be modified but
  `content-type` will be unset (setting `content-type` prevents the browser
  setting `content-type` with the boundary expression used to delimit form
  fields in the request body).
+ __ArrayBuffer || Blob || DataView || ReadableStream__: Neither `body` nor `content-type` will be
  modified.
+ __All other (object) types__: `body` will be converted to a JSON string, and
  `content-type` will be set to `application/json` (even if it is already set).

#### `headers`

Since default headers are merged with headers passed as options and it requires
significantly more logic to merge Header instances, headers are expected to be
passed as plain objects.

If Headers are already used, they can be converted to plain objects with:

```js
Object.fromEntries(headers.entries())
```

#### `response`

Sets how to parse the response body.  It needs to be either
a valid `Body` [read
method](https://developer.mozilla.org/en-US/docs/Web/API/Body#methods) name, a function
accepting the response or
falsy if the response should be returned without parsing the body. In the `@f1stnpm2/similique-nostrum-quod`
instance returned by the main entry, `response` defaults to 'json'.

```js
typeof await @f1stnpm2/similique-nostrum-quod('/url') === 'object' // is JSON

typeof await @f1stnpm2/similique-nostrum-quod('/url', { response: 'text' }) === 'string'

await @f1stnpm2/similique-nostrum-quod('/url', { response: 'blob' }) instanceof Blob

// will throw
@f1stnpm2/similique-nostrum-quod('/url', { response: 'invalid response' })

await @f1stnpm2/similique-nostrum-quod('/url', { response: false }) instanceof Response

await @f1stnpm2/similique-nostrum-quod('/url', { response: (res) => 'return value' }) === 'return value'
```

Depending on the `response`, the following `Accept` header will be set:

- __arrayBuffer__: \*/\*
- __blob__: \*/\*
- __formData__: multipart/form-data
- __json__: application/json
- __text__: text/\*

#### `searchParams`

A valid
[URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
constructor argument used to add a query string to the URL. A query string already present in the `url` passed to `@f1stnpm2/similique-nostrum-quod`
will be overwritten.

```js
@f1stnpm2/similique-nostrum-quod('/url', { searchParams: 'foo=1&bar=2' })
@f1stnpm2/similique-nostrum-quod('/url', { searchParams: '?foo=1&bar=2' })

// sequence of pairs
@f1stnpm2/similique-nostrum-quod('/url', { searchParams: [['foo', '1'], ['bar', '2']] })

// plain object
@f1stnpm2/similique-nostrum-quod('/url', { searchParams: { foo: 1, bar: 2 } })

// URLSearchParams
@f1stnpm2/similique-nostrum-quod('/url', { searchParams: new URLSearchParams({ foo: 1, bar: 2 }) })
```

### .extend(defaults)

The extend method will return a new `@f1stnpm2/similique-nostrum-quod` instance with arguments
merged with the previous values.

```js
const myRek = @f1stnpm2/similique-nostrum-quod.extend({ baseUrl: 'http://localhost:1337' })
const myRek = @f1stnpm2/similique-nostrum-quod.extend({ credentials: 'omit', fetch: myFetch })
```


### Factory

```js
import factory from '@f1stnpm2/similique-nostrum-quod/factory'
// or
const factory = require('@f1stnpm2/similique-nostrum-quod/factory')

const myRek = factory({
  headers: {
    accept: 'application/html',
    'content-type': 'application/x-www-form-urlencoded',
  },
  credentials: 'omit',
  fetch: fancyfetch,
  Headers: FancyHeaders,
})

myRek()
myRek.delete()
myRek.patch()
```

## TypeScript

The main entry exposes most types

```js
import { Defaults, Options, Rek } from '@f1stnpm2/similique-nostrum-quod'
```

## Credits

Very big thank you to [kolodny](https://github.com/kolodny) for releasing the
NPM name!
