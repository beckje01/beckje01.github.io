---
author: Jeff Beck
title: Grails Redirect Leaving %2F in URL
excerpt:
categories:
  - Java
tags:
  - apache
  - grails
  - groovy
post_format: [ ]
comments: true
date: 2012-04-03T09:56:37+0000
---
I am using a simple redirect after a user logs in so they can get to the page they were heading too the URLs were ending up with %2F in the url which caused a 404 error. The problem wasn’t actually in Grails at all but in the mod_rewrite rule being used to force HTTPS connection the rule was set up to end with [L,R] when to correctly deal with the url encoding we were already doing it needed to be [NE,L,R]


Reference: <http://www.webmasterworld.com/apache/3279075.htm>
