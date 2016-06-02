---
layout: post
title: JavaScript Design Patterns
---

We all know what design patterns are. Just do them. Here are some notes on how to do them in javascript.

# Objects in Javascript

## Creating a new Object

``` javascript
var obj = {};
var obj2 = Object.create(Object.prototype);
var obj3 = new Object();
```

## Define Property

This was introduced in ECMAScript 5.

``` javascript
Object.defineProperty(obj, 'name', {
    value: 'my name',
    writable: true,
    enumerable: true,
    configurable: true
})
```

value - the value of the property, i.e. value, function
writable - if this is false, then the property cannot be overwritten.
enumerable - if false, then the property won't appear when listing Object.keys.
configurable - allows redefining of a property. enumerable is set to false, but, configurable is set to true, then it allows for a subsequent statement to redefine the enumerable property to be set to true.

NB: be aware that defineProperty does have a performance implication.

## Inheritance Example

```javascript
// original task object
var task = {
    title: 'my title',
    description: 'my description'
};

// defining a function on task
Object.defineProperty(task, 'toString', {
    value: function() {
        return this.title + ' ' + this.description;
    }
}); 

// creating a new object that inherits from task
var urgentTask = Object.create(task);
// at this point urgent task is exactly the same as task

// now redefine the function for urgent task
Object.defineProperty(urgentTask, 'toString', {
    value: function() {
        return this.title + ' is urgent';
    }
});
// urgent task now has a different toString method

```

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

The `new` keyword

* creates a brand new object
* links to an object prototype
* binds `this` to the new object scope
* implicitly returns `this`

The drawback is that the `toString` function is duplicated for each copy of the MyObject.
An improvement would be to move the `toString` function to the prototype.
This is done by using the following format:   

> className.prototype.methodname = function(args) { ... }

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
* https://app.pluralsight.com/library/courses/javascript-practical-design-patterns