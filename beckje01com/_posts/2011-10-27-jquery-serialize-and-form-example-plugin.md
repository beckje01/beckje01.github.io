---
author: Jeff Beck
title: >
  jQuery serialize() and Form Example
  Plugin
excerpt:
categories:
  - JavaScript
  - Web Development
tags:
  - JavaScript
  - jQuery
post_format: [ ]
comments: true
date: 2011-10-27T14:32:23+0000
---
When working with jQuery 1.6.1 and the [jQuery Form Example Plugin][1] I found that the examples were being serialized the plan was to use Ajax to send the serialized data back to the server and save it but I didn’t want to have to try and remove the examples server side. So triggering unload on the examples will allow serialize to get the correct data. Example JavaScript below.
``` js
    var examples = $('.example');
    _.each(examples,function(elm){
        $(elm).unload();
    });
```

I used [Userscore.js][2] to give me a quick foreach style loop and called the unload event on every element that has the class .example which is the default class that jQuery Form Example plugin uses.






 [1]: http://mudge.github.com/jquery_example/
 [2]: http://documentcloud.github.com/underscore/
