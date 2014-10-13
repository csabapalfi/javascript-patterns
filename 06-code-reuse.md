# 6. Code reuse patterns

Prefer composition over inheritance.

## classical inheritance

* play on the word 'class', nothing to do with the word classical
* JavaScript has no classes but constructor functions make same people think that
* use the term constructor function

```js
function Parent(){ }
function Child(){ }
//inheritance magic happens here
inherit(Child, Parent);
```

* inherit is not part of the language have to implement it yourself

## classical inheritance with default pattern

```js
function inherit(Child, Parent){
    child.prototype = new Parent();
}
```
* prototype points at new parent object
* children gets parent functionality via prototype
* drawback: children gets both own and prototype properties from parent
* drawback: can't really pass parameters, or end up with a lot of objects


## the prototype chain

* can think of objects as blocks of memory
* all objects from the same constructor point at same prototype object
* can think of it as if they were pointing at it via a ```__proto__``` property
* ```__proto__``` is actually available in some JavaScript environments
* properties are looked up by walking through this prototype chain:
* if an object doesn't have a property it's ```__proto__``` is consulted

## classical inheritance with rent-a-constructor pattern

```js
function Child(a, b, c, d){
    Parent.apply(this, arguments);
}
```
* solves the problem of passing arguments
* borrows parent constructor
* passing the child object to be bound to this
* and passing any arguments
* children does not inherit prototype properties
* but gets true copies of own properties (not links)
* inheritance is just a one of action
* only copying own properties from parent
* no proto links are kept, can't access parent prototype properties
* multiple inheritance can be achieved by applying more than one constructors
* in case of multiple inheritance and duplicate properties - last one wins
