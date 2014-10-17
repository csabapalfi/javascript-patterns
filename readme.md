# 1. Introduction

## Patterns

* solution to a common problem
* don't re-invent the wheel if you don't have to
* provide a level of abstraction (models - all is wrong, some are useful)
* common patterns help communication
* this book covers
    * design patterns: (generally simpler) JS implementation of GoF patterns
    * coding pattens: JS specific pattern and good practices (main topic of the book)
    * antipatterns: common approach causing more problem than it solves

## JavaScript Concepts

### (almost) everything is an object

* only five primitive types are not objects: number, string, boolean, null, undefined
* first three has object representation with primitive wrappers
* functions are objects too
* a var is an object
    * becomes a property of the internal Activation Object or global object if it's global
    * var itself is object-like, has properties called attributes (exposed since ES5)
    * attributes determine if var can be changed, deleted or enumerated in ```for-in```

### what's an object?

* an object is simply a collection of named properties (key-value pairs)
* an object can have methods - simply when a value of a property is a function
* and object can be modified (almost) anytime - properties added, removed, updated
* an object is
    * either native: user defined (```{}```) or built-in (```Date```) -
    * native objects are described in the ES standard
    * or host object (```window```) - defined by the host environment

### no classes

* no classes, unlearn if you worked with another OO language before
* usually you just start with empty object and add properties to it
* prefer composition over inheritance

### prototypes

* one way of achieving inheritance, but nothing special
* prototype is just object property that gets added to every function
* prototype property points to a new empty object
* prototype object almost identical to object literal or new Object() but
* but constructor property points at the function you're creating not the builtin Object()

### environment

* JS programs need an environment to run
* the browser is the most common one
* most patterns in the book are environment-agnostic
* environments can provide their own host objects
    * which might have unexpected behaviour
    * not defined in the ES standard

# EcmaScript 5

* core javascript is based on the EcmaScript standard
* ES3 was accepted in 1999, ES4 was dropped, ES5 2009
* most important addition: strict mode
    * trigerred by a ```'use strict';``` - backwards compatible
    * once per scope - that function is executed in a strict subset of the language
    * error is thrown if a non strict allowed feature is used
* the plan is that future version will only support strict mode

# JSLint/JSHint

* Javascript is an interpreted language with no compiler to help spotting errors
* JSLint and JSHint are static code analysis tools
* JSLint is really strict but JSHint is more configurable
* All examples in the book pass JSLint

# console

* the ```console``` object is not part of the language but available in most environments
* Firebug in Firefox, Chrome Dev Tools in Chrome, standard I/O in Node.js, IE8+ Developer Tools
* ```console.log``` - prints all parameters passed to it
* ```console.dir``` - enumerates object and print all properties
* when testing in browser console they log output by default

# chrome dev console

* log XHR
* console.time()/console.timeEnd()
* console.dir()/dir()
* inspect() / $0
* $$
* monitorEvents() // mouse, key
* keys()/values()
* copy()

# 2. Essentials

## minimizing globals

* JavaScript uses functions to manage scope
* variables declared within a function are local and not visible outside
* global variables are declared outside of functions or used without being declared
* every JS environment has a global object - accessible via ```this``` outside of any function
* every global variable becomes a property of the global object
* in browser the global object has a property called ```window``` pointing to the global object itself

### problems with globals

* shared among all the code in your app or webpage
* danger of name collisions (especially with third-party code)
* easy to create globals involuntarily: implied globals
* implied globals: undeclared var is global
* chained declaration also results in implied globals unless vars are declared in advance
* you might accidentally overwrite existing host object properties
* globals created with var outside of any function can't be ```delete```d
* ES5 strict mode doesn't allow undeclared variables

### access to the global object

* pass a reference to this from global scope to your function
* non-strict mode: any function invoked (not with ```new```) has ```this``` pointing at the global object

### single var pattern (nope)

* the world has moved on: see [this blog post](http://benalman.com/news/2012/05/multiple-var-statements-javascript/)
* single var statement at the top of your functions
* single place to look for all the vars needed by your function
* prevents logical errors when var is used before it's defined (hoisting)
* might be good practice to initialize them with some defaults
    * to avoid undefined
    * also communicates the intended use

### hoisting

* multiple var statements (anywhere in a function) act as if they were at the top

## caching array length in for loops (?)

* for cache array length ```for (var i, max = arr.length; i < max, i++)```
* unless length can change

## for in and hasOwnProperty when needed

* for in - filter things coming down the prototype chain
```js
for (prop in obj){
    if(obj.hasOwnProperty(prop)){
    }
}
```
* unless you know and can trust the object you're dealing with

## don't augment builtin prototypes

* augmenting the prototype property of constructor functions is powerful
* but hurts maintainability when done on builtin prototypes
* when you absolutely need to: communicate with your team and check if property already exists

```js
if (typeof Object.prototype.myMethod != = function) {
    Object.protoype.myMethod = function () { // implementation
    }
}
```

## ```switch```

* align each ```case``` with ```switch```
* indent code within each ```case```
* always end ```case``` with ```break;```
* avoid fallthroughs (no break)
* end the switch with ```default:```

## avoid implied typecasting

* JavaScript implicitly typecasts variables when you compare them
* always use ```===``` and ```!===``` as they check types as well

## eval() is evil

* executing any String as JavaScript, seriously?
* use ```[]``` to access dynamic properties
* use JSON.parse for AJAX responses
* always use ```function```s with ```setTimeout``` and ```setInterval``` (instead of Strings)
* try not to use ```new Function()``` as it's similar to ```eval```

## specify radix ```parseInt()``` (?)

* ```parseInt()``` gets a numeric value from a String
* has an optional radix parameter which shouldn't be omitted (in ES3)
* ES3 ```parseInt('08')``` gets confused and treats numbers starting with 0 as octals

## code conventions

* more important to agree and consistently follow than what the exact details are
* indent everything within curly braces
* always use curly braces (even when optional: for, if)
* avoid errors or confusion by automatic semicolon insertion
    * always put opening curly braces on the same line as the previous statement
    * always use semicolons
* generic and consistent use of spaces makes code more readable
* use vertical spacing too - blank lines to separate units of code

## naming conventions

* Capitalize constructor functions to differentiate them
* use camelcase in function and variable name to separate words
* all caps for constants (not expected to change even though this can't be enforced until ES6 const)
* underscore (_ or __) prefix (and/or suffix) for private API

## write to be read

* keep comments up to date if you write them
* jsDoc and YUIDoc can generate API docs from comments

## minify in production

* any code delivered to the browser should be minified to improve performance
* Uglify, Closure Compiler, YUI Compressor

## Run JSLint/JSHint

* on edit/save

# 3. Literals

* ```{}```,```[]``` and ```//``` is preferable
* instead of using ```new Array()```, ```new Object()```, ```new RegExp()```
* more concise, less error-prone

## Object literal

* object are simple maps or key value pairs
* properties: values that are primitives or other objects
* methods: values that are functions
* custom objects (or user defined native objects) are mutable at any time
* you can add/remove functionality as you go
```js
var test = {};
test.message = 'hello';
test.hello = function(){ console.log('hello'); };
delete test.hello;
```
* but you're not required to start with an empty object
```js
var test = { message: 'hello' };
```
* note that empty object also inherit everything from ```Object.prototype```
* object literal notation emphasises that objects are just mutable hashes
* no need for classes
* also there's no need for scope resolution like with constructor
* (scope resulotion is required to look for constructor with same name in the scope chain)

## Object constructor catch

* there's no reason to use new Object() but legacy code might rely on an additional 'feature'
* Object constructor accepts a parameter
* and might delegate to another builtin constructor depending on the passed in value

## Custom constructor functions

* just a function
* when invoked with new:
    * an empty object is created referenced by ```this```
    * this inherits the prototype of the function
    * properties and method are added to ```this```
    * the newly created object referenced by ```this``` is returned
* reusable members such as methods should go to the prototype
* ```this``` is not really initalized with an empty object
* but ```this``` actually is a result of Object.create() called with prototype
* return value can be changed by explicitly returning a different object
* non-Object return values are silently ignored and ```this``` is returned

## enforcing new

* when a constructor is called without new ```this``` points at the global object
* above is no longer true in strict mode, ```this``` won't point at the global object
* always uppercase the first letter of constructor functions (and lowercase function names)
* also when not using the prototype you can just return a new object literal (```that```)

## self-invoking constructor

* another pattern to enforce new
* calls itself with new if not called with new
```js
function Waffle(){
    if(!(this instanceof Waffle)){
        return new Waffle();
        //...
    }
}
```
* alternative to above not hardcoding constructor name (not in ES5 strict mode!)
```js
if(!(this instanceof arguments.callee)){
    return new arguments.callee();
}
```
## ```arguments```

* inside every function an object called arguments is created
* containing all the parameters passed to the function when it was invoked
* arguments has a property named callee which points back at the called function
* arguments.callee is not allowed in ES5 strict mode

## Array literal

* ```[]``` is preferred to ```new Array()```
* unexpected Array constructor behaviour:
    * called with single number, uses the number as length
    * if number is a float, results in RangeError as it's not valid array length
    * hack for repeating strings: ```new Array(256).join(' '); //255 spaces```

## checking Array-iness

* ```typeof``` with array operands returns ```Object```
* before ES5 - checking for length or slice property
* ES5 introduced .isArray method
* also ```Object.prototype.toString()``` returns ```"[object Array]"``` (instead of ```"[object Object]"```)

## JSON

* combination of object and array literal notation
* but property names and Strings have to be wrapped in double-quotes
* no functions or regex literals
* ES5 JSON.parse and JSON.stringify

## Regex literal

* ```//``` is preferred to ```new RegExp()```
* shorter and no need to escape quotes or double-escape backslashes
* ```/<regex>/<flags>``` - flags (g)lobal, (m)ultiline, case (i)nsensitive
* use new RegExp() if the pattern is not known, and constructed runtime as a String
* before ES5 the literal created only one object even when called repeatedly
* (same object is a problem as it has properties like lastIndex set)
* starting from ES5 regex literal returns new object

## primitive wrappers

* JS primitives: number, string, boolean, null and undefined
* number, string and boolean have primitive wrapper
* primitive wrapper objects come with useful methods like .toFixed() or substring()
* these methods work on primitive (variables) as well, converted temporarily
* only use wrapper object when you need to augment a value and persist state (do you really need to?)
* attempting to augment primitive value doesn't result in error but won't persist state
* ```new``` wrapper constructor converts argument to primitive value

## Error object

* JS has built-in Error constructors to be used with ```throw```
* ```Error()```, ```SyntaxError()```, ```TypeError()```
* these error object have ```name``` and ```message``` properties
* Error constructors work the same way when called with or without ```new```
* ```throw``` works with any object - you can come up with custom error objects

## ```new Date()```

* one of the only builtin constructors you actually want to use

# 4. Functions

## functions are first class objects

* can be created dynamically at runtime
* can be assigned to vars, have their references copied to other vars
* can be augmented and deleted (except for some special cases)
* can be passed as arguments to other functions or returned by them
* can have their own properties and methods
* they's just another object with a special feature: they're executable
* (but don't use ```new Function(arguments, code)``` as it's bad as eval)

## functions provide scope

* in JS there's no curly braces local scope
* blocks don't create a scope
* there's only function scope
* any var defined within function is only visible in that function
* any war within an if block or for loop is local only to the wrapper function
* if there's no wrapper function then it becomes global

## terminology

* named function expression
```js
var add = function add (a,b) { ... }
```
* if you skip the name: (unnamed) function expression or anonymous function
```js
var add = function (a,b) { ... }
```
* using anonymous function won't affect definition or invocations etc.
* but the name property of the function object will be undefined
* careful, that's what's shown in stacktraces
* (the name property isn't (wasn't?) part of the ES standard)

* function declaration:
```js
function (a,b) { ... }
```
* no semicolon required for function declarations
* can only appear in program code: inside bodies of other functions or global space

## name property

* use named function expression or declaration as it sets name property
* allows function name to be displayed properly in debugger

## literal and using a different var name

* function literal is an ambiguous term and should be avoided
* giving a function a name and assigning it to a var with a different name might not work in older browsers

## function hoisting

* for function declarations the function definition also gets hoisted
* for function expressions it's only the variable that gets hoisted
* function expressions are not callable before their definitions

## callback pattern

* function reference (callback) can be passed into another function
* the other function can execute (call back) the passed in function when appropriate

## callback method vs function

* careful with using ```this``` if callback function is a method of an object
* as callback is not called as method but a function this points at either global or the callers this
* a workaround is to pass to object in a second parameter, then ```callback_function.call(obj, args)```
* or pass object and method name as string then ```var callback_function = obj[callback_name]```

## callback examples

* async event handlers in the browser accept callbacks (addEventListener)
* ```setTimeOut``` and ```setInterval``` also accepts callbacks

## returning functions

* functions are objects so they can be used as return values
* a function can return another more specialized function or create one on demand

## self defining function pattern

```js
var selfDefining = function(){
    //e.g. do some prep you only got to do once
    selfDefining = function(){
        // new function body to overwrite old one
    }
}
```

* another name for this is lazy function definition
* any properties previously set are cleared when redefining
* also if function used with a different name (assigned to another var)
* the var with the different name will use the non-redefined function

## immediate function

* execute function as soon as it's defined

```js
(function(){
    //...
}());
```

* wrapped in parens so it's clear it's not a function declaration (but expression)
* scope sandbox for initialization code
* not leaking any variables
* global objects can be passed as parameters
* so don't have to use window inside immediate function
* but don't pass too many vars to keep it readable
* scope of the immediate function can be used to store private data
* and then an object with public methods can be returned
* can be used to implement self-contained modules
* other names: self-invoking, self-executing

## immediate Object initialization

```js
({
    property: 'value'
    method: function(){ ... }
    init: function(){ ... }
}).init();
```

* wrapped in parens so that it's clear it's not a code block but object
* same purpose as immediate function above but more structured
* private helper methods are clearly distinguishable
* might not be trivial for minifiers to shorten inner helper method names
* no reference to the object is available after init
* unless you do ```return this;``` in init

## init-time branching

* also known as load-time branching
* if a condition is not going to change (e.g. browser feature)
* only check it once in your program in your init code

## function properties and memoization

* functions are objects and can have properties
* all functions have a length property (number of arguments it accepts)
* memoization: caching the return value of a function
* you can add a property named cache - object
* cache object: keys - argument, values - return value
* multiple arguments - serialize them (e.g. JSON.stringify) to get a single value

## options/configuration object

* when a function needs to be called with many parameters just use an object
* parameter or doesn't matter, optional parameters can be skipped
* easier to read, add remove parameters
* but you need to remember parameter names, older minifier might not shorten names properly

## function application

```js
var hello = function(message){ ... };
hello.apply(null, ['hey!']);
```

* a function can be applied using ```Function.prototype.apply```
* first argument is an object to bind ```this``` to
* second argument is an array of arguments
* in non-strict mode if first argument is null then it'll point at the global object
* ```Function.prototype.call``` is just syntactic sugar over apply
* call expects parameters as normal parameter list instead of an array

## function binding

* ```Function.prototype.bind``` has the same signature as ```.call```
* creates a new function bound to the object and optional parameters passed in

## partial application / currying

* call a function with less than all of it's arguments
* and return a function expecting the rest of the arguments
* this transformation of function is called currying or schonfinkelizing
* use when find yourself calling a function with same arguments

```js
// basic curry example
function curriedAdd(x,y) {
    if(y === undefined) {
        return function(y) {
            return x+y;
        }
    }
    return x + y;
}

// currying with bind
var curried = add.bind(undefined, 10);
curried(5);
```
# 5. Object creation patterns

## namespace pattern

* reduce number of globals and name collisions (without excessive prefixing)
* create a single global object for your app/lib
* change all your functions and variables to become a property of that object

```js
var myApp = myApp || {}; // prevent overwriting if namespace split across files
myApp.myFunc = function(){ ... }
```

## prevent overwriting

* prevent overwriting if namespace split across files

```js
var myApp = myApp || {};
```

## namespace function

* can create a function to turn a dot separated string to nested objects

```js
myApp.namespace = function namespace(ns){
    var parts = ns.split('.').slice(1); // split on dot, skip first item
    var parent = myApp;
    parts.forEach(part, i){
        if(typeof parent[part] === 'undefined') {
            parent[part] = {};
        }
        parent = parent[part];
    }
}

myApp.namespace('myApp.this.is.nested') === myApp.this.is.nested;
```

## declare dependencies at the top

```js
function myFunction(){
    var dom = myApp.utils.dom;
    var event = myApp.utils.event;
}
```

* shorter to type module names afterwards
* deps in one place
* some minifiers won't shorten global var names

## private members with closures

* constructor function create a closure
* any variables part of constructor closure are not visible outside of it

```js
function Person() {
    var secret = 'hey';
    return {
        doIt: function (){
            // can see secret
        }
    }
}

var me = new Person();
me.secrect === undefined;
secret === undefined;
me.doIt(); //does it
```

* privileged methods are the ones declared within the constructor
* privileged methods have access to private members
* old versions of Firefox allowed access to private scope
* internal arrays/objects are still modifiable via reference
* make sure you return a new object with only the properties needed by caller
* or copy your objects/arrays (with utility methods)

## private properties on prototype

* properties are re-created every time an object is initialized
* shared properties of prototype can also be made private with the same pattern

```js
Person.prototype = (function(){
    //all person sharing the same secret
    var secret = 'hey';
    return {
        doIt: function (){
            // can see secret
        }
    }
}());
```

## revealing module pattern

* revealing private methods by assigning them to properties of the returned object
* can name the property differently to the internal function
* can expose internal function under more than one names

```js
Person.prototype = (function(){
    function sayHello() {}
    return {
        greeting: sayHello
    }
}());
```

## module pattern

* combination of above:
    1. define a namespace
    1. assign an immediate function to it
    1. which declares dependencies at the top
    1. has private methods
    1. and returns an object revealing public API of the module
    1. globals can be passed in parameters to the immediate function
* a module can create a constructor as well
* if you return the constructor function instead of an object

## sandbox pattern

* addresses some namespace drawbacks: single global var, long dotted names

```js
new SandBox('dependencies', 'here', function(box){
  // your code here
});
```

* you have a single global constructor
* passed in callback function is you isolated environment
* you initiate multiple Sandbox object and even nest them
* you can name the constructor appropriately (instead of Sandbox)
* YUI (now dead) used this

## implementing the sandbox pattern

* as the Sandbox function is an object we can add a modules (object) property to it
* add required modules to ```this```
* then call callback with ```this```
* the callback is the users sandbox and get populated with the requested functionality

## static members

* just add property to constructor function - as that's an object as well
* (normal methods are added to the prototype or the returned object)
* you can't call static methods on instance unless you add an alias to prototype
* when aliasing static method on prototype - careful if you use ```this``` within the method

## private static members

* return constructor function via an immediate (self invoking) function
* variables within the immediate function are invisible outside (as usual)

## constants

* no real way to do it in ES5
* just naming convention: all caps, and also worth making sure they're static
* ```const``` keyword coming in ES6

## chaining

* methods (that may not have a meaningful return value) returning this
* allows calling methods in a chain in a single expression
* pros: save some typing, more concise, small and focused methods
* cons: may get a bit harder to debug - lot happening on a single line
* JQuery uses it

## ```method()``` method

* making JavaScript a bit more class-like (which is probably a bad idea)
* some syntactic sugar to add functions to the prototype property of the constructor

```js
var Person = fucntion(){...}.method('methodName', function(){ ... });
```

# 6. Code reuse patterns

Prefer composition over inheritance.

## classical inheritance

* play on the word 'class', nothing to do with the word classical
* JavaScript has no classes but constructor functions make same people think that
* use the term constructor function
* probably a bad idea but worth knowing about

## the prototype chain

* can think of objects as blocks of memory
* all objects from the same constructor point at same prototype object
* can think of it as if they were pointing at it via a ```__proto__``` property
* ```__proto__``` is actually available in some JavaScript environments
* properties are looked up by walking through this prototype chain:
* if an object doesn't have a property it's ```__proto__``` is consulted

## prototypal inheritance

* modern classless pattern
* no classes, objects inherit from objects

```js
function object (parent) {
    function F() {}
    F.prototype = parent;
    return new F();
}

var parent = { ... }
var child = object(parent);
```

* children get parent methods and properties via ```__proto__``` link
* parent can be created via constructor as well (not just literal)

## Object.create

* prototypal inheritance is built-in since ES5
* ```Object.create(parentObject, ownPropertiesObject)```

## inheritance by copying properties

```js
//shallow copy
function extend (parent, child) {
    var key;
    child = child || {};
    for(key in parent){
        if(parent.hasOwnProperty(key)){
            child[key] = parent[key];
        }
    }
    return child;
}
```

* shallow copy just copies references of arrays and object
* children can modify parent properties :(
* deep copy is when array elements and object properties are copied as well

## mixins

* just deep copy all properties from multiple objects and mix in to a new Object
* not like mixins in other languages, here: no link to parent

## borrowing methods

* sometimes you want to use one or two methods of an existing Objects
* you don't want a parent-child relationship with that Object
* can be done with ```call``` and ```apply```
* pass in your object to bind ```this``` to within the function

```js
notMyObject.doStuff.call(myObject, param1, param2);
//or
notMyObject.doStuff.apply(myObject, [param1, param2]);
```
