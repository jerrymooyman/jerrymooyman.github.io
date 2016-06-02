---
layout: post
title: Browser Rendering Optimisation
---

## The Critical Rendering Path

### Juddering

When a frame takes too long to be calculated it will be left out. When frames are left out, this causes a juddering effect.

### The Render Tree

only visible elements go into the render tree

### Layout

Layout is the process of turning the render tree and the css into the DOM.
This is also known as reflow.

### Vector to Raster

This is the process of turning an image into pixels.

## Render Pipleline

javascript -> style -> layout -> paint -> composite

if we change an elements geomentry, i.e. width
javascript -> style -> layout -> paint -> composite

if we change image background
javascript -> style -> paint -> composite

if we change applicable layers
javascript -> style -> composite

(i) different ccs properties affect the performance of a web page in different ways

css triggers web site lists information on how different properties affects a page's render pipeline.
[css trigger](https://csstriggers.com/transform)

## App Lifecycles

### LIAR

| Item | Time |
| --- | --- |
| Reponse | 100 ms |
| Animate | 16 ms |
| Idle | 50 ms |
| Load | 1 second |

### Load

Initial load should take no longer than 1 second.
This is where critial functionality should be loaded.
Load anything for above-the-fold

### Idle

Once the application has finished loading it goes into idle time. This is where the application is waiting for user input.
This is a good time to do any work that was defered to meet the 1 second load time.
Things to load during post-load idle state would be the following:

* image assets
* videos
* comments section 

Load anything for below-the-fold

### Animations

**FLIP** - First Last Invert Play

Anything that involve moment or finger on the screen requires 60fps.

### Setup Mobile Debugging

chrome://inspect

### Battling Janking

_tips for testing_

* quit other apps
* go incognito - extension / plugins can interfere
* focus on cause of bottlenecks, not symptoms
* measure first, then optimise


###JIT Compilers

[IR Hydra](http://mrale.ph/irhydra/2/)
forget micro optimisations. This should be a last resort.
Spend energy on other things instead.

###Optimizing JS Animations

javascript should be executed as soon as possible in a frame. this could trigger style calculation, layout, paint and composite

To perform animations, use requestAnimationFrame.
Old libraries used 'setTimeout' and 'setInterval'. 
(!) The javascript engine takes no notice of the rendering pipeline when scheduling these.
Not a good fit for animations.
jQuery still uses them.

So, anything that changes the layout of a page should happen inside a 'requestAnimationForm' routine.

###Long Running Javascript

[Sort Compare](http://output.jsbin.com/feloni/quiet)

###Web Workers

Web workers are a really nice way to offload processing of long running javascript onto a separate thread.

[Image manipulation Repo](https://github.com/udacity/web-workers-demo)  
[Using Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)  

###JavaScript Memory Management

[Writing Fast, Memory-Efficient JavaScript](http://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/)  
[Memory Management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)  
[High-Performance, Garbage-Collector-Friendly Code](http://buildnewgames.com/garbage-collector-friendly-code/)  

###Styleing and Layout

The performance cost of recalculating styles scales 'linearly' with the number of elements affected by the change.

A method of managing style changes by organising css is Block, Element, Modifier or BEM.
*Complex css selectors add to the browser's workload*
The take away is to keep selectors simple.

Here is some javascript that dynamically addes some boxes and then uses css selectors and records the time for each.

```javascript
var container = document.querySelector('.box-container');
var button = document.querySelector('button');
var count = 10000;

for (var i = 0; i < count; i++) {
  var div = document.createElement('div');
  div.classList.add('box');

  div.innerHTML = '<div class="title-container">' +
      '<div class="title">Box ' + (i + 1) + '</div>' +
      '</div>';

  if (i === count - 1)
    div.classList.add('box--last');

  container.appendChild(div);
}

button.addEventListener('click', function() {

  var selectors = [
    "div.box:not(:empty):last-of-type .title",
    ".box--last > .title-container > .title",
    ".box:nth-last-child(-n+1) .title"
  ];

  selectors.forEach(function(s) {
    console.time(s);
    var d = document.querySelector(s);
    console.timeEnd(s);
    console.log(d);
  });

});
```

Ways to reduce 'recalculate styles':

* reduce affected elements
    - fewer changes to the render tree
* reduce selector complexity
    - use fewer tags and class names to select elements

It can often beneficial to reduce the complexity of css selectors by using javascript.
A combination of reducing css and adding some javascript could uncover significant performance boots.


##Layout Thrashing

When running javascript that contains request to get geometric information of elements, such as <element>.offsetWidth, it forces layout calculation. This layout calculation is moved to the javascript point of the 'render pipleline'. This is ok, unless a style change is requested, such as setting the width of an element. This will cause the layout calculation to run for a second time.
If this is done in a loop, it could become very expensive operation.

This is know as **FSL** or **Forced Synchronous Layout**.

To combat FSL it is beneficial to read layout properties and then batch style updates.

Batching in this way, prevents the browser from having to redo layout calculations which can be very expensive.



this is a handy function for selecting dom elements into a collection to allow use of useful Array methods.

```javascript
function getDomNodeArray(selector) {
  // get the elements as a DOM collection
  var elemCollection = document.querySelectorAll(selector);

  // coerce the DOM collection into an array
  var elemArray = Array.prototype.slice.apply(elemCollection);

  return elemArray;
};

var divs = getDomNodeArray('div');
```

**Element**

* clientHeight, 
* clientLeft, 
* clientTop, 
* clientWidth, 
* focus(), 
* getBoundingClientRect(), 
* getClientRects(), 
* innerText, 
* offsetHeight, 
* offsetLeft, 
* offsetParent, 
* offsetTop, 
* offsetWidth, 
* outerText, 
* scrollByLines(), 
* scrollByPages(), 
* scrollHeight, 
* scrollIntoView(), 
* scrollIntoViewIfNeeded(), 
* scrollLeft, 
* scrollTop, 
* scrollWidth

**Frame, Image**

* height, 
* width

**Range**

* getBoundingClientRect(), 
* getClientRects()

**SVGLocatable**

* computeCTM(), 
* getBBox()

**SVGTextContent**

* getCharNumAtPosition(), 
* getComputedTextLength(), 
* getEndPositionOfChar(), 
* getExtentOfChar(), 
* getNumberOfChars(), 
* getRotationOfChar(), 
* getStartPositionOfChar(), 
* getSubStringLength(), 
* selectSubString()

**SVGUse**

* instanceRoot

**window**

* getComputedStyle(), 
* scrollBy(), 
* scrollTo(), 
* scrollX, 
* scrollY, 
* webkitConvertPointFromNodeToPage(), 
* webkitConvertPointFromPageToNode()


[How (not) to trigger a layout in WebKit](http://gent.ilcore.com/2011/03/how-not-to-trigger-layout-in-webkit.html)

###Managing Layers
The browser engine will place elements on layers. This is called 'update layer tree'.
Sometimes it can be beneficial to force an element into its own layer. The performance boost comes from informing the browser that the element will change.

```css
.box {
  will-change: transform
}
```
*layer promotion*

Note: only use this when appropriate. using this on elements such as text or shadows will not lead to any performance gains as paint is still required.

Update Layer Tree and Composite are not free. Aim for:

* 2 ms update layer tree
* 2 ms composite

for 60 fps critical work like animations.

**Counting Layers**
To count layers, use the TimeLine panel of the devTools.
Record some activity, ensuring you have 'Paint' capture checked. Navigate to the bottom section to view the layers.
Selecting each layer will show the reason for it being on its layer.

![alt text]({{ "/assets/images/chrome-devtool-paint-layers.png" | prepend: site.baseurl }})  

###Example Website Optimisation Walkthrough
[Test Website](http://udacity.github.io/news-aggregator/)
[Solution](https://www.youtube.com/watch?v=f_1vM3o6uJA)

## Reference

[Browser Rendering Optimization - course](https://www.udacity.com/course/browser-rendering-optimization--ud860)