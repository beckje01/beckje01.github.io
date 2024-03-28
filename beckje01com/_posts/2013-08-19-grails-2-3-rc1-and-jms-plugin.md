---
author: Jeff Beck
title: Grails 2.3 RC1 and JMS Plugin
excerpt:
categories:
  - Grails
tags:
  - grails
  - JMS
post_format: [ ]
comments: true
date: 2013-08-19T23:10:12+0000
---
Using the JMS 1.2 plugin with Grails 2.3.0.RC1 was producing a number of odd results. Mostly with missing JMS files it turns out that the new spring version didn’t have the needed spring jms included. Just add the following to BuildConfig.groovy

``` groovy
    dependencies {
      compile 'org.springframework:spring-jms:3.2.4.RELEASE'
      ...
    }
```
