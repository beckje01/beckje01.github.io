---
author: Jeff Beck
title: YUI ScriptNodeDataSource Issue
excerpt:
categories:
  - JavaScript
  - Web Development
tags:
  - JavaScript
  - JS
  - Web Development
  - YUI
post_format: [ ]
comments: true
date: 2009-03-11T22:00:50+0000"
---
While using the YUI Library, I came across an annoying issue. It is stated places that order matters when dealing with the library includes, yet there are still some issues that come up. The main one I’ve found is with the YAHOO.util.ScriptNodeDataSource this is used for accessing remote data via the YUI Get Utility. But using the configuration from Yahoo the Get library isn’t included by default, yet when you add it after finding that error the system still won’t work.

The Get Utility has to be loaded before the DataSource library, yet this is the only part of the DataSource library that require the Get utility so it isn’t listed as a dependency for that library. I used the YUI Dependency Configurator yet while working without the combined files you can still introduce the ordering issue. Yahoo needs to add a simple check that the Get utility is always loaded before the DataSource Utility yet that haven’t yet. You may not have this issue while using the combined files mode as it will all be executed at once.

So if you see an error message like “this.getUtility is undefined” make sure you have the Get Utility loaded before the DataSource.
