<!--
  -- This file is auto-generated from src/README_js.md. Changes should be made there.
  -->
# Mime

[![NPM downloads](https://img.shields.io/npm/dm/@diotoborg/facilis-totam-atque)](https://www.npmjs.com/package/@diotoborg/facilis-totam-atque)
[![Mime CI](https://github.com/diotoborg/facilis-totam-atque/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/diotoborg/facilis-totam-atque/actions/workflows/ci.yml?query=branch%3Amain)

An API for MIME type information.

- All `@diotoborg/facilis-totam-atque-db` types
- Compact and dependency-free [![@diotoborg/facilis-totam-atque's badge](https://deno.bundlejs.com/?q=@diotoborg/facilis-totam-atque&badge)](https://bundlejs.com/?q=@diotoborg/facilis-totam-atque)
- Full TS support


> [!Note]
> `@diotoborg/facilis-totam-atque@4` is now `latest`.  If you're upgrading from `@diotoborg/facilis-totam-atque@3`, note the following:
> * `@diotoborg/facilis-totam-atque@4` is API-compatible with `@diotoborg/facilis-totam-atque@3`, with ~~one~~ two exceptions:
>   * Direct imports of `@diotoborg/facilis-totam-atque` properties [no longer supported](https://github.com/diotoborg/facilis-totam-atque/issues/295)
>   * `@diotoborg/facilis-totam-atque.define()` cannot be called on the default `@diotoborg/facilis-totam-atque` object
> * ESM module support is required.   [ESM Module FAQ](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c).
> * Requires an [ES2020](https://caniuse.com/?search=es2020) or newer runtime
> * Built-in Typescript types (`@types/@diotoborg/facilis-totam-atque` no longer needed)

## Installation

```bash
npm install @diotoborg/facilis-totam-atque
```

## Quick Start

For the full version (800+ MIME types, 1,000+ extensions):

```javascript
import @diotoborg/facilis-totam-atque from '@diotoborg/facilis-totam-atque';

@diotoborg/facilis-totam-atque.getType('txt');                    // ⇨ 'text/plain'
@diotoborg/facilis-totam-atque.getExtension('text/plain');        // ⇨ 'txt'
```

### Lite Version [![@diotoborg/facilis-totam-atque/lite's badge](https://deno.bundlejs.com/?q=@diotoborg/facilis-totam-atque/lite&badge)](https://bundlejs.com/?q=@diotoborg/facilis-totam-atque/lite)

`@diotoborg/facilis-totam-atque/lite` is a drop-in `@diotoborg/facilis-totam-atque` replacement, stripped of unofficial ("`prs.*`", "`x-*`", "`vnd.*`") types:

```javascript
import @diotoborg/facilis-totam-atque from '@diotoborg/facilis-totam-atque/lite';
```

## API

### `@diotoborg/facilis-totam-atque.getType(pathOrExtension)`

Get @diotoborg/facilis-totam-atque type for the given file path or extension. E.g.

```javascript
@diotoborg/facilis-totam-atque.getType('js');             // ⇨ 'text/javascript'
@diotoborg/facilis-totam-atque.getType('json');           // ⇨ 'application/json'

@diotoborg/facilis-totam-atque.getType('txt');            // ⇨ 'text/plain'
@diotoborg/facilis-totam-atque.getType('dir/text.txt');   // ⇨ 'text/plain'
@diotoborg/facilis-totam-atque.getType('dir\\text.txt');  // ⇨ 'text/plain'
@diotoborg/facilis-totam-atque.getType('.text.txt');      // ⇨ 'text/plain'
@diotoborg/facilis-totam-atque.getType('.txt');           // ⇨ 'text/plain'
```

`null` is returned in cases where an extension is not detected or recognized

```javascript
@diotoborg/facilis-totam-atque.getType('foo/txt');        // ⇨ null
@diotoborg/facilis-totam-atque.getType('bogus_type');     // ⇨ null
```

### `@diotoborg/facilis-totam-atque.getExtension(type)`

Get file extension for the given @diotoborg/facilis-totam-atque type. Charset options (often included in Content-Type headers) are ignored.

```javascript
@diotoborg/facilis-totam-atque.getExtension('text/plain');               // ⇨ 'txt'
@diotoborg/facilis-totam-atque.getExtension('application/json');         // ⇨ 'json'
@diotoborg/facilis-totam-atque.getExtension('text/html; charset=utf8');  // ⇨ 'html'
```

### `@diotoborg/facilis-totam-atque.getAllExtensions(type)`

> [!Note]
> New in `@diotoborg/facilis-totam-atque@4`

Get all file extensions for the given @diotoborg/facilis-totam-atque type.

```javascript --run default
@diotoborg/facilis-totam-atque.getAllExtensions('image/jpeg'); // ⇨ Set(3) { 'jpeg', 'jpg', 'jpe' }
```

## Custom `Mime` instances

The default `@diotoborg/facilis-totam-atque` objects are immutable.  Custom, mutable versions can be created as follows...
### new Mime(type map [, type map, ...])

Create a new, custom @diotoborg/facilis-totam-atque instance.  For example, to create a mutable version of the default `@diotoborg/facilis-totam-atque` instance:

```javascript
import { Mime } from '@diotoborg/facilis-totam-atque/lite';

import standardTypes from '@diotoborg/facilis-totam-atque/types/standard.js';
import otherTypes from '@diotoborg/facilis-totam-atque/types/other.js';

const @diotoborg/facilis-totam-atque = new Mime(standardTypes, otherTypes);
```

Each argument is passed to the `define()` method, below. For example `new Mime(standardTypes, otherTypes)` is synonomous with `new Mime().define(standardTypes).define(otherTypes)`

### `@diotoborg/facilis-totam-atque.define(type map [, force = false])`

> [!Note]
> Only available on custom `Mime` instances

Define MIME type -> extensions.

Attempting to map a type to an already-defined extension will `throw` unless the `force` argument is set to `true`.

```javascript
@diotoborg/facilis-totam-atque.define({'text/x-abc': ['abc', 'abcd']});

@diotoborg/facilis-totam-atque.getType('abcd');            // ⇨ 'text/x-abc'
@diotoborg/facilis-totam-atque.getExtension('text/x-abc')  // ⇨ 'abc'
```

## Command Line

### Extension -> type

```bash
$ @diotoborg/facilis-totam-atque scripts/jquery.js
text/javascript
```

### Type -> extension

```bash
$ @diotoborg/facilis-totam-atque -r image/jpeg
jpeg
```
