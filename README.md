# snapdragon [![NPM version](https://badge.fury.io/js/snapdragon.svg)](http://badge.fury.io/js/snapdragon)

> snapdragon is an extremely pluggable, powerful and easy-to-use parser-renderer factory.

* Bootstrap your own parser, get sourcemap support for free
* All parsing and rendering is handled by simple, reusable middleware functions
* Inspired by the parsers in [Jade](http://jade-lang.com)and [CSS](https://github.com/reworkcss/css).

## Install

Install with [npm](https://www.npmjs.com/)

```bash
npm i snapdragon --save
```

## Usage examples

```js
var snapdragon = require('snapdragon');

// custom parser middleware
var ast = snapdragon.parser('some string')
  .use(foo())
  .use(bar())
  .parse();

var res = snapdragon.renderer(ast);
  .set('foo', function () {})
  .set('bar', function () {})
  .render()
```

See the [examples](./examples/)

* [basic](./examples/basic-example.js)
* [filepath](./examples/filepath-example.js)
* [sourcemaps](./examples/sourcemaps.js)

Try running all three examples from the command line. Just do `node examples/sourcemaps.js` etc.

## Getting started

**Parsers**

Parsers are middleware functions used for parsing a string into an ast node.

```js
var ast = snapdragon.parser(str, options)
  .use(function() {
    var pos = this.position();
    var m = this.match(/^\./);
    if (!m) return;
    return pos({
      // `type` specifies the renderer to use
      type: 'dot',
      val: m[0]
    });
  })
```

**AST node**

When the parser finds a match, `pos()` is called, pushing a node onto the ast that looks something like:

```js
{ type: 'dot',
  val: '.',
  position:
   { start: { line: 1, column: 1 },
     end: { line: 1, column: 2 } }
```

**Renderers**

Renderers are middleware functions that visit over an array of ast nodes to render a string.

```js
var res = snapdragon.renderer(ast)
  .set('dot', function (node) {
    console.log(node.val)
    //=> '.'
    return this.emit(node.val);
  })
```

_(TBC)_

## TODO

* [ ] getting started
* [ ] docs for `.use`
* [ ] docs for `.set`
* [ ] docs for `nodes` (recursion)
* [ ] unit tests

## Related projects

Snapdragon was inspired by these great projects by [tj](https://github.com/tj):

* [css](https://github.com/reworkcss/css): CSS parser / stringifier
* [jade](http://jade-lang.com): A clean, whitespace-sensitive template language for writing HTML

## Running tests

Install dev dependencies:

```bash
npm i -d && npm test
```

## Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/snapdragon/issues/new)

## Author

**Jon Schlinkert**

+ [github/jonschlinkert](https://github.com/jonschlinkert)
+ [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

## License

Copyright (c) 2015 Jon Schlinkert
Released under the MIT license.

***

_This file was generated by [verb-cli](https://github.com/assemble/verb-cli) on May 10, 2015._