---
layout: post
title: JavaScript Promises
categories:
- javascript
---

These are some notes on JavaScript promises according to Kyle Simpson.

# The Problem  
Classic ajax calls use callbacks to deal with a returning request.  
This has the following issues:  
1. Control of your application has been inverted. A callback function is provided which is called by the request handling code. You pass control over and trust that the request handling code does the right thing, i.e.
 * invoke your callback  
 * invoke it only once  
 * etc.  
2. You invariably need to perform some action when your ajax call resolves. If these actions in turn require a further ajax call, you create a series of nested callbacks. This creates an anti pattern known as callback hell.  

# References
[](https://blog.getify.com/promises-part-1)
