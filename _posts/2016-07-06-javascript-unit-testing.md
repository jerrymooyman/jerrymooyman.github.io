---
layout: post
title: JavaScript Unit Testing
categories:
- javascript
- unit testing
---
* TOC
{:toc}

# Setup
The current preference is the build and run unit test using the following:  
* mocha  
* chai  
* babel-register  

These dependencies should already be part of the package.json so a simple 'npm install' should download everything that is needed.  
Also, we use babel-register to enable writing tests in ES6.  

The following scripts added to package.json are required to run tests.  
```javascript
{
  ...
  "scripts": {
    ...
    "test": "mocha --compilers js:babel-register --recursive",
    "test:watch": "npm test -- --watch",
  },
  ...
}
```

Now, just run 'npm test' or 'npm run test:watch'.  


# References  
[Writing Tests with Redux](http://redux.js.org/docs/recipes/WritingTests.html)  
[The ultimate unit testing cheat sheet](https://gist.github.com/yoavniran/1e3b0162e1545055429e)  
[Test Driven React Tutorial](http://spencerdixon.com/blog/test-driven-react-tutorial.html)  

# Appendix  
Sublime Test snippet for creating new test modules
