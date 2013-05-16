---
layout: post
title: "debugging and testing windows service applicaitons: a better approach"
date: 2013-04-23 16:55
comments: true
categories: 
---
One of the major boring part of working with windows service applications is to debug and test it. every time we need to install the service to test or debug it. also it is impoissible to hold the execution with a breakpoint in the onstart method of service. 
We can apply some tricks to cover this.
<!-- more -->
Running as a console application:
---------------------------------
The application can be tested as a console application in development time by changing the constructor.

{% gist 5441890 %}


Creating a seperate windows form app for debugging:
---------------------------------------------------

We can add a seperate windows form project in the solution dedicated to testing. the main application will be developed as a seperate projects which will have start, stop, pause methods.
A seperate project will be created for implementing windows service. this service will call corresponding start,stop methods of the main application. another win form app will also call those methods at its button events.

Debugging existing windows service appication:
-----------------------------------------------
however existing windows service applications can be debugged by running the service after installing and attach the process from source code.