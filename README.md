@spacek33z/ref-wchar-napi
==========
### A ref "type" implementation of `wchar_t *` (a.k.a. wide string) backed by "iconv-lite"
[![Build Status](https://secure.travis-ci.org/SpaceK33z/ref-wchar-napi.svg)](https://travis-ci.org/SpaceK33z/ref-wchar-napi)
[![Build Status](https://ci.appveyor.com/api/projects/status/v5ej12uqw1u0379j?svg=true)](https://ci.appveyor.com/project/SpaceK33z/ref-wchar-napi)

This module offers a ["wide
strings"](http://en.wikipedia.org/wiki/Wide_character#C.2FC.2B.2B) (`wchar_t *`)
implementation on top of Node.js Buffers using the ref "type" interface. Supports Node 6, 7, 8, 10, 12.

**This is a fork of ref-wchar-napi, which uses iconv. This fork uses iconv-lite, which is as implied, is way liter.*


Installation
------------

Install with `npm`:

``` bash
$ npm install @spacek33z/ref-wchar-napi
```


Examples
--------

Say you have a C library that exports a `wchar_t` char and a `wchar_t *` wide
string:

``` c
#if defined(WIN32) || defined(_WIN32)
#define EXPORT __declspec(dllexport)
#else
#define EXPORT
#endif

EXPORT whcar_t w = L'W';

EXPORT wchar_t s[] = L"hello world";

EXPORT wchar_t **str = (wchar_t **)(&s);
```

``` js
var ref = require('ref-napi');
var dlfcn = require('dlfcn');
var wchar_t = require('@spacek33z/ref-wchar-napi');
var wchar_string = wchar_t.string;

var lib = dlfcn('./libexample');

// get the "w" symbol as a wchar
var w = lib.get('w', wchar_t.size);
ref.get(w, wchar_t);
// "W"

// get the "s" symbol as a wide string
var s = ref.reinterpretUntilZeros(lib.get('s'), wchar_t.size);
wchar_t.toString(s);
// "hello world"

// get the "str" pointer symbol as a wide string
var str = lib.get('str', wchar_string.size);
ref.get(str, wchar_string);
// "hello world"
```


License
-------

(The MIT License)

Copyright (c) 2014 Nathan Rajlich &lt;nathan@tootallnate.net&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
