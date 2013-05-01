# Tower Style Guide

## JavaScript

- For assigned functions, have no whitespace between parentheses and brackets: `){` vs. `) {`.
    ```js
    // true
    exports.init = function(){
      this.x;
    };
    
    // false
    exports.init = function() {
      this.x;
    };
    ```
- For callback functions, do the same thing.
    ```js
    // true
    route('/users')
      .on('exec', function(){

      });
    
    // false
    route('/users')
      .on('exec', function() {

      });
    ```
- For non-assigned functions, do it the other way around:
    ```js
    // true
    function init() {
      this.x;
    };
    
    // false
    function init(){
      this.x;
    };
    ```
    The reason for this is readability and for the mind to quickly grasp what type of function it is. It's subtle but it makes the code much easier to grasp for some strange reason.
- Indent DSL methods if they are in a new object scope.
    ```js
    // true
    adapter('facebook')
      .model('user')
        .attr('username')
        .action('create')
        .action('find')
          .param('page', 'integer')
      .model('like');
    
    // false
    adapter('facebook')
      .model('user').attr('username').action('create')
      .action('find')
      .param('page', 'integer')
      .model('like');
    ```
- Keep 1 space between native methods and parentheses (`if`, `while`, `for`, etc.).
    ```js
    // true
    if (x) y();

    // false
    if(x) y();
    ```
- Emit event names in the present tense as `verb [object|property]`:
    ```js
    // true
    this.emit('define', user);
    this.emit('define user', user);
    
    // false
    this.emit('defined user', user);
    this.emit('user defined', user);
    this.emit('user define', user);
    ```
- Emit namespaced events from most generic to most specific. This way, you can mixin more generic stuff first, and use those generic methods on specific objects.
    ```js
    // true
    this.emit('define', user);
    this.emit('define facebook', user);
    this.emit('define facebook.user', user);
    
    // false
    this.emit('define facebook.user', user);
    this.emit('define facebook', user);
    this.emit('define', user);
    ```
- Pass the `this` as the first parameter in DSL methods, instead of using `this`. It makes it more explicit what the context actually is:
    ```js
    // true
    route('/users')
      .on('request', function(ctx, next){
        ctx.render();
      });

    // false
    route('/users')
      .on('request', function(next){
        this.render();
      });
    ```
- Place the compared value on the left instead of the right. This is unnatural at first, but it makes the code much easier to read for a newcomer.
    ```js
    // true
    if ('string' === typeof(x))
      exec();

    // false
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
     * true
     */

    exports.init = function(){

    };
    ```
    ```js
    /**
     * false
     */
    exports.init = function(){

    };
    ```

- For loops should be as efficient and clean as possible. Even though the performance benefit most of the time is extremely minimal if at all.

   ```javascript
   for (var i = 0, n = arr.length; i < n; i++) {}
   ```

   Single letter variables should only be used within loops (basically for loops).

- For each loops (for loops but with objects) should have safe guards, which is a general good JavaScript practice.
  
   ```javascript
   for (var key in obj) {
     if (obj.hasOwnProperty(key)) {
  
     }
   }
   ```
- Use `obj.init()` instead of `new Obj`, where `obj.create()` should do some db/service call that actually tries to save/insert/create/POST a record.
- For single-line arrays and objects, put one space before/after containing brackets.
    ```js
    // true
    [ 'i386', 'x86_64' ]
    { status: 'active' }

    // false
    ['i386', 'x86_64']
    {status: 'active'}
    ```

## Events

These are all of the events used in Tower. When creating custom APIs, see if these events can be used before defining another. Maybe we can even limit it down.

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

## Examples

- https://github.com/component/struct/blob/master/index.js
- https://github.com/component/pipe/blob/master/index.js
- https://github.com/component/raf/blob/master/index.js
- https://github.com/component/to-function/blob/master/index.js
- https://github.com/component/enumerable/blob/master/index.js
- https://github.com/component/network
- https://github.com/component/trace/blob/master/index.js
- http://stackoverflow.com/questions/5495984/coding-style-guide-for-node-js-apps