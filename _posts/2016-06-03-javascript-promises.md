---
layout: post
title: JavaScript Promises
categories:
- javascript
---

These are some notes on JavaScript promises according to Jake Archibald and Kyle Simpson.

# The Problem  
Classic ajax calls use callbacks to deal with a returning request.  
This has the following issues:  

1. Control of your application has been inverted. A callback function is provided which is called by the request handling code. You pass control over and trust that the request handling code does the right thing, i.e.   
     * invoke your callback  
     * invoke it only once  
     * etc.  
2. You invariably need to perform some action when your ajax call resolves. If these actions in turn require a further ajax call, you create a series of nested callbacks. This creates an anti pattern known as 'the pyramid of death' or callback hell.  

# Promises

Here is how you create one

``` javascript
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```

Here is how you use the above promise

``` javascript
promise.then(function(result) {
  console.log(result); // "Stuff worked!"
}, function(err) {
  console.log(err); // Error: "It broke"
});
```

# Converting jQuery Deferreds into Standard Promises  
Fortunately, the javascript promise API will play nicely with other promise libraries. If an object has a `then` method on it, it will be treated like a standard promise.  

In the case of jQuery Deferred, we can use the following:  

``` javascript
var jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

NB: be aware that standard promises will return only one argument. In libraries, such as jQuery where multiple arguments are returned, only the first argument will be available.

NB: jQuery will not return an error object into rejection callbacks.

# Chaining  
Chaining is done by chaining multiple `then`s after each other to transform results or perform additional async actions.

Transforming  

``` javascript
get('story.json').then(function(response) {
  return JSON.parse(response);
}).then(function(response) {
  console.log("Yey JSON!", response);
});
```

# Multiple Async Actions in Sequence  
If your `then` method returns a value, that value is passed into the scope of the subsequent `then` method.  
If your `then` method returns a promise, the next `then` method will wait for that promise to settle.  

This is some cool example code  

``` javascript
var storyPromise;

function getChapter(i) {
  storyPromise = storyPromise || getJSON('story.json');

  return storyPromise.then(function(story) {
    return getJSON(story.chapterUrls[i]);
  })
}

// and using it is simple:
getChapter(0).then(function(chapter) {
  console.log(chapter);
  return getChapter(1);
}).then(function(chapter) {
  console.log(chapter);
});
```

`story.json` is downloaded only once. However the promise that is returned can be `then`'d multiple times after the fact.

# When There are Errors  
There are 2 usable forms  

``` javascript
// example 1
get('story.json').then(function(response) {
  console.log("Success!", response);
}, function(error) {
  console.log("Failed!", error);
});

//example 2
get('story.json').then(function(response) {
  console.log("Success!", response);
}).catch(function(error) {
  console.log("Failed!", error);
});
```

NB: `.catch` is equivilant to `.then(undefined, function(error) { ...`

It is important to note, that a promise will only call the `fulfilled` or `rejected` function, never both.  
If no `rejection` method is present, then control will skip forward to the next `then` in the chain with a rejection callback.  

# JavaScript Exceptions  
When javascript exceptions occur inside a promise constructor, the promise implicitly rejects.  

This gives the added benefit of additional error handling, so it pays to do promise related work inside the promise constructor callback.  

This is also the case for exceptions that occur inside `then` methods.

# Thinking about things in a sequence  

``` javascript
// Start off with a promise that always resolves
var sequence = Promise.resolve();

// Loop through our chapter urls
story.chapterUrls.forEach(function(chapterUrl) {
  // Add these actions to the end of the sequence
  sequence = sequence.then(function() {
    return getJSON(chapterUrl);
  }).then(function(chapter) {
    addHtmlToPage(chapter.html);
  });
});
```

In this case, we start with a `resolved` promise. We can then chain `then`s in order the iterate an array sequentially.

A tidier way of doing the above is to use Array.prototype.reduce function. We just pass it a `resolve`d promise as the initial value. 

``` javascript
// Loop through our chapter urls
story.chapterUrls.reduce(function(sequence, chapterUrl) {
  // Add these actions to the end of the sequence
  return sequence.then(function() {
    return getJSON(chapterUrl);
  }).then(function(chapter) {
    addHtmlToPage(chapter.html);
  });
}, Promise.resolve());
```

# Multiple async calls concurrently
Rather than calling async methods sequentially, one after the other, we can make all our async calls simultaneously and then process then sequentially when the last one has arrived.  

``` javascript
Promise.all(arrayOfPromises).then(function(arrayOfResults) {
  //...
});
```

This example maps an array of promises and then reduces them to chain promises together in order to process then in order.  

``` javascript
getJSON('story.json').then(function(story) {
  addHtmlToPage(story.heading);

  // Map our array of chapter urls to
  // an array of chapter json promises.
  // This makes sure they all download parallel.
  return story.chapterUrls.map(getJSON)
    .reduce(function(sequence, chapterPromise) {
      // Use reduce to chain the promises together,
      // adding content to the page for each chapter
      return sequence.then(function() {
        // Wait for everything in the sequence so far,
        // then wait for this chapter to arrive.
        return chapterPromise;
      }).then(function(chapter) {
        addHtmlToPage(chapter.html);
      });
    }, Promise.resolve());
}).then(function() {
  addTextToPage("All done");
}).catch(function(err) {
  // catch any error that happened along the way
  addTextToPage("Argh, broken: " + err.message);
}).then(function() {
  document.querySelector('.spinner').style.display = 'none';
});
```


# Promise API  

**Promise.resolve(promise);**  
Returns promise (only if promise.constructor == Promise)  

**Promise.resolve(thenable);**  
Make a new promise from the thenable. A thenable is promise-like in as far as it has a "then" method.  

**Promise.resolve(obj);**  
Make a promise that fulfills to obj. in this situation.  

**Promise.reject(obj);**  
Make a promise that rejects to obj. For consistency and debugging (e.g. stack traces), obj should be an instanceof Error.  

**Promise.all(array);**  
Make a promise that fulfills when every item in the array fulfills, and rejects if (and when) any item rejects. Each array item is passed to Promise.resolve, so the array can be a mixture of promise-like objects and other objects. The fulfillment value is an array (in order) of fulfillment values. The rejection value is the first rejection value.  

**Promise.race(array);**  
Make a Promise that fulfills as soon as any item fulfills, or rejects as soon as any item rejects, whichever happens first.  

# References
[](https://blog.getify.com/promises-part-1)  
[JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/)  
[ECMA Script Language Definition](https://tc39.github.io/ecma262/#sec-promise-objects)  
