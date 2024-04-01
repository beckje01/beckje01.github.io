---
author: Jeff Beck
title: Grails 2.3.1 Warnings after clean.
excerpt:
categories:
  - Grails
  - Software Engineering
tags:
  - grails
  - groovy
post_format: [ ]
comments: true
date: 2013-10-23T09:02:02+0000
---
If you are running grails 2.3.1 and see the following sequence pop up before you get some odd test failures.

``` sh
    $ grails clean
    | Application cleaned.

    $ grails test-app
    | Environment set to test.....
    | Warning No config found for the application.
    | Warning DataSource.groovy not found, assuming dataSource bean is configured by Spring
```
Start using package in between and the problem will go away.

``` sh
    $ grails clean
    | Application cleaned.
    $ grails package
    | Compiling 10 source files
    | Compiling 12 source files.....

    $ grails test-app
    | Environment set to test.....
    | Server running. Browse to http://localhost:8080/api
    | Running 6 cucumber tests...
    | Completed 6 cucumber tests, 0 failed in 0m 3s
    | Server stopped
    | Tests PASSED
```
