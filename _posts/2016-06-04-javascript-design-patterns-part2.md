---
layout: post
title: JavaScript Design Patterns: Structural
---

These patterns either  
 * extend functionality
 * simplify functionality

# Decorator Pattern  

``` javascript
var Task = function(name) {...}

Task.prototype.complete = function() {...}
Task.prototype.save = function() {...}

var myTask = new Task('my task')
myTask.complete()
myTask.save()

// now we create a new task and decorate it with a new function
var notifyingTask = new Task('new task')
notifyingTask.notify = function() {...}

notifyingTask.save = function() {
    this.notify()
    Task.prototype.save.call(this)
}
```

In the above example, we decorate our `notifyingTask` with a notify function and we call it when we save our task.  
Note how we extend the `save` function to call `notify()` and then `save()` on the prototype.

NB: `Object.call(this)` is a way to call a method on a prototype in the context of the current object.

# Facade Pattern

# Flyweight

# Behavioral Design Patterns

## Observer Pattern

## Mediator Pattern

## Command Pattern


# References
[Practical Design Patterns in JavaScript](https://app.pluralsight.com/library/courses/javascript-practical-design-patterns)  
