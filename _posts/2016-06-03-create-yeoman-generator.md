---
layout: post
title: Create Your Own Yeoman Generator
categories:
- javascript
- yeoman
---

# Yeoman Generators  
There are many generators available for scafolding your web application with Yeoman.  These are very opinionated and may not fit your project needs perfectly.  
In this case, you will need to do additional work to add any missing packages and remove any packages that you won't be using. Each additional thing you need to do, takes away from the benefits of using a generator in the first place.  

When you get to a point where the benefits are no longer obvious, you may think about writing you own generator.  

Yeoman provides documentation to do just that.

In addition, you could use the generator built by Yeoman to help you get started. Its a generator to generate generators...

# Generator-Generator  
[Generator-Generator Repo](https://github.com/yeoman/generator-generator)  

## Setup  
``` shell
> npm install -g generator-generator
> yo generator
```

Project Structure  

``` shell
├── generators/  
│   └── app/    
│       ├── index.js  
│       └── templates/  
│           └── dummyfile.txt  
├── .editorconfig  
├── .gitattributes  
├── .gitignore    
├── .eslintrc  
├── .travis.yml  
├── .yo-rc.json  
├── package.json  
├── gulpfile.js  
├── README.md  
├── LICENSE  
└── test/  
    └── app.js  
```

# References  
[Authoring](http://yeoman.io/authoring/)  
[Yeoman Generator-Generator](https://github.com/yeoman/generator-generator)  