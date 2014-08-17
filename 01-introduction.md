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
