---
author: Jeff Beck
title: Remove Addressbar from iPhone Web App
excerpt:
categories:
  - Software Engineering
  - Web Development
tags:
  - HTML
  - iPad
  - iPhone
  - Mobile
post_format: [ ]
comments: true
date: 2010-06-22T16:55:31+0000
---
If you are creating a web app for the iPhone there is an easy to to hide the address bar when opening the page from a link on the users home screen. You simply need to add the following meta tag to your  
head. This will also work with the iPad.

``` xml
<meta name="apple-mobile-web-app-capable" content="yes" />
```
