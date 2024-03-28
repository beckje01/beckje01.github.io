---
author: Jeff
title: wget and special characters
excerpt:
layout: post
categories:
  - Software Engineering
tags:
  - Software Engineering
  - wget
post_format: [ ]
comments: true
date: 2008-11-24T01:36:15+0000
---
While using wget with authorization to download some data you may run into the problem of having a special character such as ‘>’ in your password. It’s a simple fix of wrapping your password in quotes. But the only error message that this problem will give is an authentication failure.
