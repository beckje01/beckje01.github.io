---
author: Jeff Beck
title: Check for Address Bar in iOS
excerpt:
categories:
  - JavaScript
  - Web Development
tags:
  - iOS
  - iPad
  - iPhone
  - JavaScript
  - Mobile
  - Web Development
post_format: [ ]
comments: true
date: 2011-02-08T20:07:11+0000
---
As we know how to hide the address bar in iOS, we may want to tell if a user has that bar showing or not. I use this detection to show instructions to the user to add a site to their home screen if they are not in stand alone mode.

There are two properties we want to look at *“standalone” in window.navigator* and *window.navigator.standalone*. We can use the first expression to check if the device supports full screen mode and the second to check if it is in full screen mode. I have checked these with the iPhone and iPad, with iOS versions 3 and 4 if anyone has any issues please post a comment and we can try and work through it.

The following checks will after the page is loaded output to the console the mode its in. From there you can easily replace the console.log with what ever functions you would like.

<script src="https://gist.github.com/beckje01/926010.js"></script>

There is a great write up by Ben Nadel you can find over at his blog. [Post][1]


 [1]: http://www.bennadel.com/blog/1950-Detecting-iPhone-s-App-Mode-Full-Screen-Mode-For-Web-Applications.htm
