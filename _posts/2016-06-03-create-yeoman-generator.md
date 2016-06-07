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
npm install -g generator-generator
yo generator
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

# Manual

## Setup

Start by just initialising a new node app. Lets call it `generator-jam-react`. By convention, a generator name should start with `generator`.

Accept all the default. Not important at this stage.

``` shell
npm init
```

``` shell
npm install yeoman-generator --save
```

The yeoman-generator is the base package required to build a custom generator.

## Creating a generator template

Create an `app` folder. Inside that, create an `index.js` file. As specified in the package.json config file, this will be the entry point of our generator application.

NB: By convention, the yeoman-generator will look for the `app` folder.

Place the following code in the `index.js` file

``` javascript
'use strict'

let generators from 'yeoman-generator'

export default generators.Base.extend({
    myMethod: function() {
        this.log('hello, world!')
    }
})
```

This is the standard convention for setting up a generator.

## Registering the generator with the local environment

Normally, you would install a yeoman generator using `npm`. In order to run a generator locally, we need to tell npm to also look in the local project directory for the generator.

We do this by creating a link

``` shell
npm link
```

This is run from the project directory and basically creates a symbolic link.
Now you can run `yo` and you should see the new generator in the list of available generators.

# Working with the file system

**File Contexts**

* destination - where the application is scaffolded
* template - where templates are stored

## Static files  

Create a `templates` directory in the `app` directory. This will be the home for all our template files.

Now place a favicon.ico file into the templates directory. As a convention, prefix all template files with `_` to denote that its a source file.  
So, our favicon.ico should be _favicon.ico.

# Running the Generator

Create a new directory and run the generator.  

``` shell
mkdir test-app && cd test-app
yo jam-react
```

![Yeoman run]({{ "/assets/img/yeoman-run1.png" | prepend: site.baseurl }})  

**Yeoman API methods** 

|this.log(message) | log to stdout |
|this.templatePath() | the location where our templates live |
|this.destinationPath() | the location where our files will be written to |
|this.copy(source, destination) | copies source to destination |
|this.directory(source, destination) | copies entire source directory to destination |




# References  
[Authoring](http://yeoman.io/authoring/)  
[Yeoman Generator-Generator](https://github.com/yeoman/generator-generator)  
[npm link](https://docs.npmjs.com/cli/link)  
