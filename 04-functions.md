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
