---
layout: post
title: Clone an existing repository into a new repository
categories:
- blog
---
![alt text]({{ "/assets/images/git-tricks.png" | prepend: site.baseurl }})  

This is useful if you want to start a new project using an existing repo as a slingshot (starting point). For this example, I have a repo with some basic scaffolding which I will use to start other projects from.  

1) Create a new github repository: jam-resume  

2) Clone existing repo into a new directory  

``` shell 
$ git clone https://github.com/jerrymooyman/howto-react-with-webpack.git jam-resume      
```

3) Now, edit the git config file and point your repo to a new origin  

``` shell 
$ cd jam-resume  

    [remote "origin"]  
    fetch = +refs/heads/*:refs/remotes/origin/*  
    url = https://github.com/jerrymooyman/jam-resume.git  
```

4) Push your changes  

``` shell
$ git push origin master  
```

### References
[Fork your own project on GitHub](http://bitdrift.com/post/4534738938/fork-your-own-project-on-github)