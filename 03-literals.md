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
