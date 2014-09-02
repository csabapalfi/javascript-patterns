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
* function literal: - an ambiguous term which can mean named or unnamed function expression
