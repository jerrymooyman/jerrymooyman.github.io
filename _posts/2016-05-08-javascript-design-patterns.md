---
layout: post
title: JavaScript Design Patterns
---

# Creational Design Patterns

## Constructor Pattern

``` javascript
var MyObject = function (param1, param2) {
    this.param1 = param1;
    this.param2 = param2;
    this.toString = function () {
        return this.param1 + ' ' + this.param2;
    }
}

var object = new MyObject('hello', 'world!');
```

The new keyworkd

* creates a brand new object
* links to an object prototype
* binds 'this' to the new object scope
* implicityly returns 'this'

The drawback is that the 'toString' function is duplicated for each copy of the MyObject.
An improvement would be to move the 'toString' function to the prototype.
This is done by using the following format: 
className.prototype.methodname = function(args) { ... }

``` javascript
MyObject.prototype.toString = function() {
    return this.param1 + '' + this.param2;
}
```

## Module Pattern

## Factory Pattern

## Singleton Pattern

# Structural Design Patterns

## Decorator Pattern

## Facade Pattern

## Flyweight

# Behavioral Design Patterns

## Observer Pattern

## Mediator Pattern

## Command Pattern


# References
