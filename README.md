# disinfect
[![Build Status](https://travis-ci.org/genediazjr/disinfect.svg?branch=master)](https://travis-ci.org/genediazjr/disinfect)
[![Coverage Status](https://coveralls.io/repos/github/genediazjr/disinfect/badge.svg?branch=master)](https://coveralls.io/github/genediazjr/disinfect?branch=master)
[![Code Climate](https://codeclimate.com/github/genediazjr/disinfect/badges/gpa.svg)](https://codeclimate.com/github/genediazjr/disinfect)
[![NPM Version](https://badge.fury.io/js/disinfect.svg)](https://www.npmjs.com/disinfect)
[![NPM Downloads](https://img.shields.io/npm/dt/disinfect.svg?maxAge=2592000)](https://www.npmjs.com/disinfect)<br>
[![Dependency Status](https://david-dm.org/genediazjr/disinfect.svg)](https://david-dm.org/genediazjr/disinfect)
[![Known Vulnerabilities](https://snyk.io/test/github/genediazjr/disinfect/badge.svg)](https://snyk.io/test/github/genediazjr/disinfect)
[![NSP Status](https://nodesecurity.io/orgs/genediazjr/projects/57a4e17d-bf27-4150-b763-3c92b244d2c5/badge)](https://nodesecurity.io/orgs/genediazjr/projects/57a4e17d-bf27-4150-b763-3c92b244d2c5)

Hapi plugin to apply Google's [Caja](https://github.com/google/caja) HTML Sanitizer on route query, payload, and params.

* Capable for custom sanitization and per-route configuration.
* Can also be used for input formatting using the custom sanitizer option.
* Can be disabled per route.

## Usage

```js
const registerPlugins = async (server) => Promise.all([
    server.register({
        plugin: require('disinfect'),
        options: {
            disinfectQuery: true,
            disinfectParams: true,
            disinfectPayload: true
        }
    })
]);

registerPlugins(server)
    .then(() => {
        // ...
    })
    .catch((err) => {
        // ...
    })

```
[Glue](https://github.com/hapijs/glue) manifest
```js
register: {
    plugins: [
        {
            plugin: require('disinfect'),
            options: {
                disinfectQuery: true,
                disinfectParams: true,
                disinfectPayload: true
            }
        }
    ]
}
```

## Options

* **deleteEmpty** - remove empty query or payload keys.
* **deleteWhitespace** - remove whitespace query, payload, or params keys.
* **disinfectQuery** - sanitize query strings.
* **disinfectParams** - sanitize url params.
* **disinfectPayload** - sanitize payload.
* **genericSanitizer** - custom synchronous function to do the sanitization of query, payload, and params.
* **querySanitizer** - custom synchronous function to do the sanitization of query strings.
* **paramsSanitizer** - custom synchronous function to do the sanitization of url params.
* **payloadSanitizer** - custom synchronous function to do the sanitization of payload.

`deleteEmpty` and `deleteWhitespace` defaults to `false`.

`disinfectQuery`, `disinfectParams`, and `disinfectPayload` defaults to `false`. If set to true, object will be passed to `caja` first before custom sanitizers.

```
dirtyObject ->`Caja` sanitizer -> `genericSanitizer` -> `query-`, `params-`, or `payload-` sanitizer -> deleteWhitespace -> deleteEmpty -> cleanObject.
```

`genericSanitizer`, `querySanitizer`, `paramsSanitizer`, and `payloadSanitizer` should be in the following format:

```js
const customSanitizer = (dirtyObj) => {
    // ...
    return cleanObj;
}
```

All options can be passed on a per-[route](http://hapijs.com/api#route-options) basis. Route options overrides server options.

```js
// example
{
    path: '/',
    method: 'get',
    handler: (request, reply) => {
        ...
    },
    options: {
        plugins: {
            disinfect: {
                disinfectQuery: true,
                disinfectParams: false,
                disinfectPayload: true
            }
        }
    }
}
```

Disable on a route.
```js
{
    path: '/',
    method: 'get',
    handler: (request, reply) => {
        ...
    },
    options: {
        plugins: {
            disinfect: false
        }
    }
}
```

## Contributing
* Include 100% test coverage
* Follow the [Hapi coding conventions](http://hapijs.com/styleguide)
* Submit an issue first for significant changes.

## Credits
* [hapi-sanitize-payload](https://github.com/lob/hapi-sanitize-payload) - Hapi plugin to sanitize the request payload
* [Caja-HTML-Sanitizer](https://github.com/theSmaw/Caja-HTML-Sanitizer) - Bundles Google Caja's HTML Sanitizer within a npm installable node.js module
