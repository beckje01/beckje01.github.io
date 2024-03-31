---
author: Jeff Beck
title: Read only tables with IE6/IE7
excerpt:
categories:
  - JavaScript
tags:
  - IE6
  - IE7
  - JavaScript
  - JS
post_format: [ ]
comments: true
date: 2007-11-11T14:23:46+0000
---
Finding a fix for the dynamic table row problem, I started to use the insertRow() functionality which did solve the problem in FireFox. So I would call insertRow() on the table then returned in the table row object that I then used innerHTML to add all the relevant code to produce the inputs and labels. This was tested and worked great in FF, this type of solution was wanted instead of adding each cell because there were Ajax calls used to generate some of the drop down inputs.

When it came to testing with IE6 and IE7 here came the problem there was a strange error when ever the rows were being added. So I did some digging and I finally found the following.

> **Note **In Internet Explorer, the innerText and innerHTML properties are read-only on the tr object.

This came from MSDN, available <a href=http://msdn2.microsoft.com/en-us/library/ms532998.aspx">here</a>. So great now I’m faced with finding a workaround or redeveloping all the Ajax based menus. I could switch to JSON or returning JavaScript and running eval on it, but that would mean either redoing all the static parts of the system or having two separate pieces of code to do the same thing which is never good.

So if your going to be adding dynamic rows to tables with inputs I’ve ruled out using innerHTML on both the table element and the tr. Check back soon and I’ll see if I can find a workaround or if I end up using a different method to generate my rows.

**Update**  
View a workaround [here][1]


 [1]: http://blog.jeffbeck.info/?p=16
