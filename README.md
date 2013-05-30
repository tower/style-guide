# Tower Style Guide

## General Documentation Semantics

- Write in a conversational style.
  - Use pronouns: _I, we, you, they_.
  - Use colloquial expressions: _a sure thing, turn-on, rip-off, OK_.
  - Use contractions: _they're, you're, it's, here's, we've, I'm_.
  - Use simple words.
  - If you must choose between writing naturally and being grammatically correct, write naturally.

  ```
  // yay
  I'm going home.
  It's simple.

  // nay
  I am going home.
  I is simple.
  ```

## JavaScript
- Define `var` one at a time (don't use leading/trailing commas):
    ```js
    // yay
    var event = require('event');
    var dom = require('dom');
    var assert = require('assert');

    // nay
    var event = require('event'),
        dom = require('dom'),
        assert = require('assert');

    // nay
    var event = require('event')
      , dom = require('dom')
      , assert = require('assert');
    ```
    While the later two are minimally more optimized, minifiers like uglifyjs will make these optimizations for you. So write what is most human readable.
- Use trailing commas for multi-line arrays.
    ```js
    // yay
    var events = [
      'click',
      'keypress',
      'focusin',
      'focusout'
    ];

    // nay
    var events = [
        'click'
      , 'keypress'
      , 'focusin'
      , 'focusout'
    ];
    ```
- Use trailing commas for multi-line objects.
    ```js
    // yay
    var styles = {
      color: '#000',
      background: '#fff',
      width: 100,
      height: 200
    };

    // nay
    var styles = {
        color: '#000'
      , background: '#fff'
      , width: 100
      , height: 200
    };
    ```
- Always use `"` double quotes when declaring JSON keys and values.
    ```js
    // yay
    {
      "myKey": "string",
      "myArray": ["some stuff", "more stuff"]
    }
   
    // nay
    {
      myKey: 'string',
      'myArray': ['some stuff', 'more stuff']
    }
    ```

- For strings that are not defined in JSON, default to declaring strings with `'` single quotes. In the case where your string contains special format characters or `'` single quotes, use `"` double quotes.
    ```js
    // yay
    var str = '"Marty" is at the party!';
    var specialStr = "im'oto";

    // nay
    var str = "\"Marty\" is at the party!";
    var specialStr = 'im\'oto';
    ```
- For assigned functions, have no whitespace between parentheses and brackets: `){` vs. `) {`.
    ```js
    // yay
    exports.init = function(){
      this.x;
    };
    
    // nay
    exports.init = function() {
      this.x;
    };
    ```

- For callback functions, do the same thing.
    ```js
    // yay
    route('/users')
      .on('exec', function(){

      });
    
    // nay
    route('/users')
      .on('exec', function() {

      });
    ```
- For non-assigned functions, do it the other way around.
    ```js
    // yay
    function init() {
      this.x;
    };
    
    // nay
    function init(){
      this.x;
    };
    ```
    This distinguishes the function declarations and function expressions.
- Indent DSL methods if they are in a new object scope.
    ```js
    // yay
    adapter('facebook')
      .model('user')
        .attr('username')
        .action('create')
        .action('find')
          .param('page', 'integer')
      .model('like');
    
    // nay
    adapter('facebook')
      .model('user').attr('username').action('create')
      .action('find')
      .param('page', 'integer')
      .model('like');      
    ```
- Keep 1 space between native methods and parentheses (`if`, `while`, `for`, etc.).
    ```js
    // yay
    if (x) y();

    // nay
    if(x) y();
    ```
- Emit event names in the present tense as `verb [object|property]`.
    ```js
    // yay
    this.emit('define', user);
    this.emit('define user', user);
    
    // nay
    this.emit('defined user', user);
    this.emit('user defined', user);
    this.emit('user define', user);
    ```
- Emit namespaced events from most generic to most specific. This way, you can mixin more generic stuff first, and use those generic methods on specific objects.
    ```js
    // yay
    this.emit('define', user);
    this.emit('define facebook', user);
    this.emit('define facebook.user', user);
    
    // nay
    this.emit('define facebook.user', user);
    this.emit('define facebook', user);
    this.emit('define', user);
    ```
- Pass the `this` as `context` for the first parameter in DSL methods, instead of using `this` as a reference. The context is stated more clearly.
    ```js
    // yay
    route('/users')
      .on('request', function(context, next){
        context.render();
      });

    // nay
    route('/users')
      .on('request', function(next){
        this.render();
      });
    ```
- Place the compared value on the left instead of the right. This is unnatural at first, but it makes the code much easier to read for a newcomer.
    ```js
    // yay
    if ('string' === typeof(x))
      exec();

    // nay
    if (typeof(x) === 'string')
      exec();
    ```
    The reason for this is, the compared-to value (e.g. `'string'`) is first, so it reads `if ('string') exec()`.
- Leave 1 empty line at the **top* of each file.
    ```js
    \n
    /**
     * Blank line above.
     */
    ```
    ```js
    /**
     * No blank line above
     */
    ```
- Leave 0 empty lines at the **bottom** of each file.
- Leave 1 blank line between comments and code.
    ```js
    /**
     * GOOD
     */

    exports.init = function(){

    };
    ```
    ```js
    /**
     * BAD
     */
    exports.init = function(){

    };
    ```

- For loops should be as efficient and clean as possible.

   ```js
   // yay
   for (var i = 0, n = arr.length; i < n; i++) {}

   // nay
   for (var i = 0; i < arr.length; i++) {}
   ```

   Single letter variables should only be used within loops (basically for loops).
  ```js
   // yay
   for (var i = 0, n = arr.length; i < n; i++) {}

   // nay
   for (var index = 0, size = arr.length; index < size; index++) {}
   ```

- For each loops (for loops but with objects) should have safe guards, which is a general good JavaScript practice.
  
   ```js
   // yay
   for (var key in obj) {
     if (obj.hasOwnProperty(key)) {
       delete obj[key];
     }
   }

   // nay   
   for (var key in obj) {
     delete obj[key];
   }
   ```
- Use `obj.init()` instead of `new Obj`, where `obj.create()` should do some db/service call.
    ```js
    // yay
    var newObj = obj.init();

    // nay
    var newObj = new Obj();
    ```
- For single-line arrays and objects, put one space before/after containing brackets.
    ```js
    // yay
    [ 'i386', 'x86_64' ]
    { status: 'active' }

    // nay
    ['i386', 'x86_64']
    {status: 'active'}
    ```
- Avoid aligning signs such as `=`, `:`, etc. The reason is, while it does create some symmetry, it makes the code a little harder to read.
    ```js
    // yay
    var x = 1;
    var foo = 'bar';
    var hello = 'world';

    // nay
    var x     = 1;
    var foo   = 'bar';
    var hello = 'world';
    ```
- If a ternary statement is too long, put it on multiple lines with `?` and `:` at the start of the line.
    ```js
    // yay
    exports.query = function(name){
      return null == name
        ? query().start(this.className)
        : query(name);
    }

    // nay
    exports.query = function(name){
      return null == name ?
        query().start(this.className) :
        query(name);
    }
    ```    
- If you are building a multiline string, use `+` at the beginning of the line.
    ```js
    // yay
    var string = 'some '
      + 'long '
      + 'string ';

    // nay
    var string = 'some ' +
      'long ' +
      'string ';

    // nay
    var string = 'some \
      long \
      string ';
    ```
    In EcmaScript 6, you'll be able to use [backticks for multi-line strings](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml?showone=Multiline_string_literals#Multiline_string_literals).

## Repos

- Name your projects using lowercase with hyphens.
    ```js
    // yay
    var myProject = require('my-project');

    // nay
    var myProject = require('myProject');
    var myProject = require('MyProject');
    ```

## Events

These are all of the events used in Tower. When creating custom APIs, see if these events can be used before defining another. Maybe we can even cut it down.

```js
emit('define');
emit('init');
emit('exec');
emit('open');
emit('close');
emit('connect');
emit('disconnect');
emit('render');
emit('request');
emit('error');
emit('data');
```

Maybe `route.on('request')` becomes `route.on('exec')`. And `route.on('connect')` becomes `route.on('open')`.

## Tests

- Use easy to scan/understand syntax for assertions.
    ```js
    // yay
    assert(true === val);
    assert(false === val);
    assert('foo' === val);
    assert(undefined === val);

    // nay
    assert.isTrue(val);
    assert.isFalse(val);
    assert.equal('foo', val);
    assert.isUndefined(val);
    // - should syntax
    val.should.be(undefined);
    ```

## Comments

- Here is the general structure of a good comment:
    ```js
    /**
     * Iterate each value and invoke `fn(val, i)`.
     *
     *    users.each(function(val, i){
     *
     *    });
     *
     * @param {Function} fn
     * @return {Object} self
     * @api public
     */

    proto.forEach = function(fn){
      // ...
      return this;
    };
    ```

## Readme Structure

The basic format:

- Title
- Installation
- Examples (optional)
- API
- Running Tests
- Contributing
- Notes (optional)
- License

<pre><code># Title

Quick description (1-2 sentences).

## Installation

node.js:

```
npm install tower-project
```

browser:

```
component install tower/project
```

## Example

```js
var project = require('tower-project');

// ...
```

## API

Optional brief intro.

### .on(event, fn, [capture])

Short description of some method:

```js
dom('a.remove').on('click', function(e){

});
```

### .on(event, selector, fn, [capture])

Bind delegate `event` handler function for `selector`:

```js
dom('ul li').on('click', 'a.remove', function(e){

});
```

## Running Tests

Install testem:

```bash
$ npm install -g testem
```

Run tests:

```bash
$ testem
```

Then, open all the browsers you want to test by going to the outputted url defaulted to [http://localhost:7357](http://localhost:7357)

Tests will run on any open browser linked to the stated url and your current node environment.

## Contributing

Before you send a pull request, make sure your code meets the style guidelines at [https://github.com/tower/style-guide](https://github.com/tower/style-guide) and all tests pass.

## Notes

This section is optional, but where you'd write whatever, 
like design decisions, when to use, overview, etc. 
Here you could go into the theory too (distributed data, etc.).

## Licence

MIT
</code></pre>

## Examples

- https://github.com/component/struct/blob/master/index.js
- https://github.com/component/pipe/blob/master/index.js
- https://github.com/component/raf/blob/master/index.js
- https://github.com/component/to-function/blob/master/index.js
- https://github.com/component/enumerable/blob/master/index.js
- https://github.com/component/network
- https://github.com/component/trace/blob/master/index.js
- http://stackoverflow.com/questions/5495984/coding-style-guide-for-node-js-apps