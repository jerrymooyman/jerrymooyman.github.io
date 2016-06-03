---
layout: post
title: Slingshot Webapps with Yeoman Generators
categories:
- blog
- javascript
---

# The Value of Setting up a new Project  
It is very important to go through the process of setting up new projects from scratch. It helps to understand what role the various components play in the overall scheme of you web application.  

It also forces you to think about which components to use when dealing with concerns such as routing, templating, etc. Through experience, you naturally form opinions.  

Once you have gone through this exercise serveral times, you will naturally look for ways to fast-track the process.

# What is Yeoman?  
> Yeoman helps you to kickstart new projects, prescribing best practices and tools to help you stay productive.  

The premise is that you use a project generator, which is basically a template, to do your project setup. From here, you should be able to just start developing you application.

One thing to be aware of, is that these generators are very opinionated. They have to be. So be sure you find the generator that best fits your project needs.  

## Installation

``` shell
> npm install -g yo
```

Install it globally. This will only need to be done once.  

# Generators

[Here](http://yeoman.io/generators/) is a list of generators.  

# This is a list of my favourite genertors  
This will be a growing list and I will add to it as I use new ones.

## React-Webpack-Redux

``` shell
> npm install -g generator-react-webpack-redux
```

### Project setup  

``` shell
# Create a new directory, and `cd` into it:
mkdir my-new-project && cd my-new-project

# Run the generator
yo react-webpack-redux

```

# References  
[Yeoman](http://yeoman.io/)  
[generator-react-webpack-redux](https://github.com/stylesuxx/generator-react-webpack-redux)  

