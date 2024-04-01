---
author: Jeff Beck
title: Filling an Array with Objects in PHP
excerpt:
categories:
  - Software Engineering
  - Web Development
tags:
  - PHP
  - Web Development
post_format: [ ]
comments: true
date: 2010-01-28T07:29:45+0000
---
While working in PHP I found the need to fill an array with new objects on the fly. So at first I tried the array_fill function that worked fine for values but when I tried an object I found it was the same object in every position meaning if I modified one they all changed. What I needed was a new object in each. The easiest way I found to do that was a for loop.

The following doesn’t work correctly


```php
$arr = array_fill(1,12,new MyObject());
```
So to accomplish the same idea I did the following


``` php
     $arr=array();

     for($i=1;$i<=12;$i++)
     {
          //Fill Array with new object
          $arr[$i]=new MyObject();
     }
```
