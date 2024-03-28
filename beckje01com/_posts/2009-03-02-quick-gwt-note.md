---
author: Jeff
title: Quick GWT Note
excerpt:
layout: post
categories:
  - Java
  - Web Development
tags:
  - Google Web Toolkit
  - GWT
  - Java
post_format: [ ]
comments: true
date: 2009-03-02T20:41:56+0000
---
While working on a small GWT project with two other developers I ran into an odd runtime exception “com.google.gwt.user.client.rpc.IncompatibleRemoteServiceException” as I was testing my app in the hosted mode I’m not sure how to refresh that built in browser which is the listed fix. What I ended up doing was a simple clean of the project which solved my problem, I think it relates to how I’m using SVN not integrated into Eclipse with the project yet its hard to tell. But if you run accross this expection try a simple clean before pulling too much hair out.
