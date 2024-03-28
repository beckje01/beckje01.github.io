---
author: Jeff Beck
title: Broken Dynamic Tables
excerpt:
layout: post
categories:
  - JavaScript
  - Software Engineering
tags:
  - JavaScript
  - JS
post_format: [ ]
comments: true
date: 2007-11-08T10:34:28+0000
---
I’ve been building a web based application that uses Ajax and other JavaScript extensively. While working with adding rows to tables dynamically I’ve found the wrong way to do it and thought I should share. When adding rows to tables if the rows are simply data this method will work fine, but when the rows contain form inputs there is a problem.

This will add a new row no problem.


``` js
    var elm = document.getElementById("tableone");
    var old = elm.innerHTML;
    elm.innerHTML = old + "<tr><td>Sample</td></tr>";
```
But if we add some inputs we start getting problems.

``` js
    var elm = document.getElementById("tableone");
    var old = elm.innerHTML;
    count++;
    elm.innerHTML = old + "<tr><td><input type='text' name='test"+count+"' id='text"+count+"' /></td></tr>";
```

When we add one row then enter a value in the input then, the user hit a plus button to add another row the data that was entered is lost. So this is how not to do dynamic tables with inputs, more to follow on how to fix this problem.
