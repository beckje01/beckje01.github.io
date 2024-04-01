---
author: Jeff Beck
title: java.lang.IncompatibleClassChangeError
excerpt:
categories:
  - Java
  - Software Engineering
tags:
  - Java
  - Software Engineering
post_format: [ ]
comments: true
date: 2008-09-24T13:13:38+0000
---
So I encountered this odd error while working with the Crystal Reports XI Java Report Viewer. It showed up as “java.lang.IncompatibleClassChangeError: Implementing class” this has been one of the more cryptic messages from Java that I’ve had in a while. So a quick search turned up that this error can occur when parts of the Java Project have been compiled at separate times. So I did a quick clean build, well not that quick. Unfortunately I still got the error at run time. It was only happening with Crystal Reports so with some more digging I finally found that ReportViewer.jar and jrcerom.jar must be the first two jars in the class path to work. While I this is the first time in Java I’ve seen this it wasn’t too hard to fix. While using NetBeans in the library tab there are buttons to change the order of the files, unfortunately if you aren’t using NetBeans you will have go in and change the class path files by hand.  
