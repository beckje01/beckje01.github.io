---
author: Jeff Beck
title: >
  Grails Display the Current Spring
  Security Filter Chain
excerpt:
categories:
  - Grails
tags:
  - grails
  - Spring Secruity
post_format: [ ]
comments: true
date: 2013-01-15T10:52:37+0000
---
While working with Grails and the Spring Security plugin, the current spring security filter chain is available in the *springSecurityFilterChain* bean. It is very easy with that to show what the current chain looks like so you can work through filter chain issues. I used the following code in the [Grails Console][1] plugin to get the bean:


``` groovy
def filterChain = ctx.getBean('springSecurityFilterChain')
```
Also if you want to poke around the other beans available this is a great post to check out: [Spring Beans from the Grails Console][2] .

 [1]: http://grails.org/plugin/console
 [2]: http://www.redtoad.ca/ataylor/2012/05/spring-beans-from-the-grails-console/
