# Functions

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
