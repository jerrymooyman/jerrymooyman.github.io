---
layout: post
title: Yeoman, there's no need to feel down
categories:
- javascript
- yeoman
---
These are some notes about creating your own Yeoman generator.

* TOC
{:toc}

# Yeoman Generators  
There are many generators available for scafolding your web application with Yeoman.  These are very opinionated and may not fit your project needs perfectly.  
In this case, you will need to do additional work to add any missing packages and remove any packages that you won't be using. Each additional thing you need to do, takes away from the benefits of using a generator in the first place.  

Or you could write your own generator.

Yeoman provides documentation to do it. There are also may tutorials on how to do it.

# Creating a Generator

## Installation

Start by just initialising a new node app. Lets call it `generator-jam-react`. By convention, a generator name should start with `generator`.

Accept all the default. Not important at this stage.

``` shell
npm init
```

``` shell
npm install yeoman-generator --save
```

The yeoman-generator is the base package required to build a custom generator.

## Setup

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

## File Contexts

* destination - where the application is scaffolded
* template - where templates are stored

## Static files  

Create a `templates` directory in the `app` directory. This will be the home for all our template files.

Now place a favicon.ico file into the templates directory. As a convention, prefix all template files with `_` to denote that its a source file.  
So, our favicon.ico should be _favicon.ico.

## Running the Generator

Create a new directory, cd into it and run the generator.  

``` shell
mkdir test-app && cd test-app
yo jam-react
```

You should see the following output.

![Yeoman run]({{ "/assets/img/yeoman-run1.png" | prepend: site.baseurl }})  

## Adding a template

Add a _index.html file to the templates directory. This will be a typical SPA index.html file.

Update the title tag in the header as follows:

``` html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>`<%= appname %>`</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

The important thing to notice is that we have placed an ejs (effective javascript templating) directive into the title tag. This is where our generator will inject a value into.

Add the following to our generator:  

``` javascript
html: function() {
    this.fs.copyTpl(
        this.templatePath('_index.html'),
        this.destinationPath('src/index.html'),
        {
            appname: 'my awesome app'
        }
    );
}
```

Running the generator will now yield the following result:

* index.html file has been copied into the src directory
* templating engine has injected the title `my awesome app` into the title tag of our index.html.

![Yeoman template]({{ "/assets/img/yeoman-template1.png" | prepend: site.baseurl }})  

## Dynamic file generation

Configuration files, such as bower.json, need to be generated dynamically.
Here is an example

``` javascript
bower: function() {
    var bowerJson = {
        name: 'my-awesome-app',
        license: 'MIT',
        dependencies: {}
    };
    bowerJson.dependencies['packagename'] = '~1.2.3';
    ... etc
    this.fs.writeJSON('bower.json', bowerJson);
}

```

## Other artifacts

``` javascript
packageJSON: function(){
    this.fs.copy('_package.json', 'package.json');
},

git: function(){
    this.fs.copy('gitignore', '.gitignore');
}

```

NB: Note that, by convention, files that would normally start with a `.` would be named without the `.` in the source.

# User Input

## YoSay & Chalk

[YoSay](https://github.com/yeoman/yosay) is a utility to get the Yeoman character to say something.  
[Chalk](https://github.com/chalk/chalk) is a utility to do colourful log messages.

``` shell
npm install --save yosay chalk
```

Once installed add require statements to our generator and add some code into the prompting method.

``` javascript
prompting: function() {
    this.log(yosay('Welcome to JAM React Generator!'));
},
```

We should get the following output with the Yeoman character saying our message.

![Yeoman yosay]({{ "/assets/img/yeoman-yosay.png" | prepend: site.baseurl }})  

## Arguments

In the constructor of our generator we can add the following code:

``` javascript
constructor: function () {
    generators.Base.apply(this, arguments);
    this.argument('appname', {
        type: String,
        required: true
    });
},
```

`this.argument` receives an argument name with some properties (as json), which will be accessable via `this.appname`.

`this.appname` is in the `this` scope so it can be used throughout our other functions as required, such as injecting the appname as the title in our index.html.

## Options

Options are used as decision points in our generator application. These are setup in the constructor.  
We can use an option to decide whether Yeoman should say our custom message or not.

``` javascript
// constructor
this.option('includeyosay', {
    desc: 'make the yeoman say our message',
    type: Boolean,
    default: false
});

// prompting function
if(this.options.includeyosay) {
    this.log(yosay('Welcome to JAM React Generator!'));
}
```

We run our generator without the `--includeyosay` switch.

![Yeoman yosay]({{ "/assets/img/yeoman-option1.png" | prepend: site.baseurl }})  

Now with the `--includeyosay` switch.

![Yeoman yosay]({{ "/assets/img/yeoman-option2.png" | prepend: site.baseurl }})  


# Yeoman API methods  

| method | description
| ---- | ---- | 
| this.log(message) | log to stdout | 
| this.templatePath() | the location where our templates live | 
| this.destinationPath() | the location where our files will be written to | 
| this.copy(source, destination) | copies source to destination | 
| this.directory(source, destination) | copies entire source directory to destination | 

# Generator-Generator  
[Generator-Generator Repo](https://github.com/yeoman/generator-generator)  

In addition, you could use the generator built by Yeoman to help you get started. Its a generator to generate generators...

Installation

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


# References  
[Authoring](http://yeoman.io/authoring/)  
[Yeoman Generator-Generator](https://github.com/yeoman/generator-generator)  
[npm link](https://docs.npmjs.com/cli/link)  
