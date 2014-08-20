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
* inspect()
* $$
* monitorEvents() // mouse, key
* keys()/values()
* copy()
