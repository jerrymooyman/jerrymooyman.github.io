---
layout: post
title: Chrome Dev Tools
---
![Chrome Dev Tools]({{ "/assets/img/chrome-developer-tools.jpg" | prepend: site.baseurl }})  

* TOC
{:toc}

Here are some notes I took from a [Chrome Developer Tools course on PluralSight](https://app.pluralsight.com/library/courses/chrome-developer-tools/table-of-contents)  

## The Console

Console logging levels  

* Log, Info, Debug
* Warn
* Error
* Assert  

<br>
![alt text]({{ "/assets/img/console-log-levels.png" | prepend: site.baseurl }})  

### Console.Assert

```javascript
console.assert(true)
```

A console assert that is true will be ignored.  
A console assert that is false will show an assertion error

### Grouping and Formatting

**Grouping**  
A group is started with 'group' and closed with 'groupEnd'. Any log lines between the start and end of the group will be grouped together.  
Groups can also be nested.

![alt text]({{ "/assets/img/javascript-log-groups.png" | prepend: site.baseurl }})  

![alt text]({{ "/assets/img/console-log-groups.png" | prepend: site.baseurl }})  

**Formatting options**

* %s - string
* %d/%i - integer
* %f - float
* %o - DOM element
* %c - styled with CSS

The above formatting options can be used to format log message.  

<br>
*javascript*  
![alt text]({{ "/assets/img/javascript-formatting.png" | prepend: site.baseurl }})  

*console output*  
![alt text]({{ "/assets/img/console-formatting.png" | prepend: site.baseurl }})  

CSS styles can be applied to console log message.

*javascript*  
![alt text]({{ "/assets/img/javascript-log-css-style.png" | prepend: site.baseurl }})  

*console output*  
![alt text]({{ "/assets/img/console-log-css-style.png" | prepend: site.baseurl }})  

**Logging Objects**

```javascript
console.log(document)
```
This will log the html of the object to the console.

![alt text]({{ "/assets/img/console-log-document.png" | prepend: site.baseurl }})  

```javascript
console.dir(document)
```
This will log the javascript notation of the object to the console.

![alt text]({{ "/assets/img/console-log-document-javascript.png" | prepend: site.baseurl }})  

<br>
**Timing**  
Timing is specified by using the following lines

```javascript
console.time('step time')
...
... other lines of code
...
console.timeEnd('step time')
```
The timer will keep track of the time it took to get from 'time' to 'timeEnd'  
This could be really useful for debugger javascript to find what is taking a large amount of time.

<br>
**Selectors**

* $(selection) - single css selector
* $$(selection) - collection css selector
* $x(xpath) - 

<br>
**Inspect**  

```javascript
inspect($('#chromeImage'))
```
This will navigate to the matching element on the Elements tab. It is a really useful way of finding elements directly in the source.

<br>
**Monitoring Events**  

```javascript
monitorEvents($('h1'), 'mouse')
```
With this command, all mouse events triggered on the matching 'h1' element will be logged to the console.

![alt text]({{ "/assets/img/console-monitor-events.png" | prepend: site.baseurl }})  

This is a list of the event types
![alt text]({{ "/assets/img/chrome-devtools-event-types.png" | prepend: site.baseurl }})  


## References  
[Chrome DevTools Official Documentation](https://developers.google.com/web/tools/chrome-devtools/)