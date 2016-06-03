---
layout: post
title: Create Your Own Yeoman Generator
categories:
- javascript
- yeoman
---

# Yeoman Generators  
There are many generators available for scafolding you web application with Yeoman.  These are very opinionated and may not fit your project needs perfectly.  

So, why not create your own generator?

Yeoman provides documentation to do just that.

Another option is to use the generator built by Yeoman to help you get started. Its a generator to generate generators...

# Generator-Generator  
[Repo](https://github.com/yeoman/generator-generator)  

## Setup  
``` shell
> npm install -g generator-generator
> yo generator
```

*Project Structure*  
``` shell
    .  
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
[Yeoman Generator-Generator](https://github.com/yeoman/generator-generator)  
[Authoring](http://yeoman.io/authoring/)