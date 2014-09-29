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
