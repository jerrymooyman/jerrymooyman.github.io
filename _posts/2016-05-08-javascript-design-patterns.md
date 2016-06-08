---
layout: post
title: JavaScript Design Patterns - Creational
---
* TOC
{:toc}

We all know what design patterns are. Just do them. Here are some notes on how to do them in javascript.

# Objects in Javascript

## 3 Ways of Creating a new Object

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

* value - the value of the property, i.e. value, function 
* writable - if this is false, then the property cannot be overwritten.
* enumerable - if false, then the property won't appear when listing Object.keys.
* configurable - allows redefining of a property. enumerable is set to false, but, configurable is set to true, then it allows for a subsequent statement to redefine the enumerable property to be set to true.

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

# Constructor Pattern

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
    return this.param1 + ' ' + this.param2;
}
```

So, in `Eric Elliot's` talk at Fluent Conf 2013, creating objects using the `new` keyword is the old and busted way.  
This is now the prefered hot way of doing it:  

``` javascript
var MyObject = {
    toString: function() {
        return this.param1 + ' ' + this.param2;
    }
}
var myObject = Object.create(MyObject)
myObject.param1 = 'hello'
myObject.param2 = 'world'

```

# Module Pattern

The module pattern is a way to organise related code into a cohesive unit. In this example, the repo module contains only database related functions.  

Also, it is important to note that due to the way javascript closures work, private variables, such as `db` in this case, are not accessible to anything outside of the module.

``` javascript
// repo module

var repo = function() {
    
    var db = {}

    var get = function(id) {
        return {
            name: 'new task from database'
        }
    }    

    var save = function(task) {
        console.log('saving task to database')
    }

    // this is the revealing module pattern
    return {
        get: get,
        save: save
    }
}

module.exports = repo();
```

``` javascript
// task.js
var Repo = require('./repo')

Task.prototype.save = function() {
    // passes this to the save function of Repo which in this case will
    // be the task
    Repo.save(this)
}
```

``` javascript
// main.js
var Task = require('./task')
var Repo = require('./repo')

// uses the repo to fetch a database record with id 1 to populate a Task object
var task1 = new Task(Repo.get(1))
// calls save, which will internally call save on Repo passing task as 
// the argument
var task1.save()
```

# Factory Pattern  

 * simplifies object creation
 * create different objects based on need

``` javascript
// repo factory
var repoFactory = function() {

    this.getRepo = function(repoType) {
        if(repoType === 'task') {
            var taskRepo = require('./taskRepo')()
            return taskRepo
        }
        if(repoType === 'user') {
            var userRepo = require('./userRepo')()
            return userRepo
        }
    }
}

module.exports = new repoFactory;
```

``` javascript
// main.js
var repoFactory = require('./repoFactory')

var task1 = new Task(repoFactory.getRepo('task').get(1))
```

You can see by this example that the logic for setting up and determining the required repo has been deferred to the repoFactory module.

# Singleton Pattern

``` javascript
var TaskRepo = (function() {
    var taskRepo;
    function createRepo() {
        var taskRepo = new Object('Task')
        return taskRepo
    }
    return {
        getInstance: function() {
            if(!taskRepo) {
                taskRepo = createRepo()
            }
            return taskRepo
        }
    }
})();

var repo1 = TaskRepo.getInstance()
```


# References
[Practical Design Patterns in JavaScript](https://app.pluralsight.com/library/courses/javascript-practical-design-patterns)  
