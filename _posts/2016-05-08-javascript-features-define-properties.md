---
layout: post
title: JavaScript Features - Define Property
---

This was introduced in ECMAScript 5.

```JavaScript
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


