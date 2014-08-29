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
