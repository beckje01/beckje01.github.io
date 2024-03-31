---
author: Jeff Beck
title: Java and JavaScript Datetime Fun
excerpt:
categories:
  - Java
  - JavaScript
  - Web Development
tags:
  - Java
  - JavaScript
  - JS
  - Web Development
post_format: [ ]
comments: true
date: 2011-05-08T12:52:26+0000
---
So dealing with Datetime between client and server side can always be fun due to timezones but adding a number of browsers makes it all the more complicated. For our project we are using [Joda-Time][1] on the Java side and a mix of technologies on the client side (I will be addressing that soon). So we deiced to pass all datetimes via ISO-8601 the problem came when dealing with the different browsers. IE7 and IE8 both output a string that doesn’t have the milliseconds where Chrome and FireFox did.

On the Java side we used [ISODateTimeFormat.dateTime()][2] to parse the incoming date string so we wanted the milliseconds.

To make this work I made a quick JavaScript utility that I can use to output the correct format in UTC time as we wanted. You can see the code below.  

<!-- more -->

Its rather simple just using the UTC versions of getting the date and time elements I construct a string that will be ISO-8601 formatted. I have tested this in IE 7, 8, and 9 Chrome and FireFox. If there are any issues feel free to comment and I will do what I can to help.

<script src="https://gist.github.com/beckje01/961617.js"></script>

 [1]: http://joda-time.sourceforge.net/
 [2]: http://joda-time.sourceforge.net/apidocs/org/joda/time/format/ISODateTimeFormat.html#dateTime()
