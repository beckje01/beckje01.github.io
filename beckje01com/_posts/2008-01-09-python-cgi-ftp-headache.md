---
author: Jeff
title: Python CGI + FTP = Headache
excerpt:
layout: post
categories:
  - Python
  - Software Engineering
tags:
  - FTP
  - Python
post_format: [ ]
comments: true
date: 2008-01-09T09:30:16+0000
---
I have taken over support of a Python CGI application, no big deal seems like an ok code base and the fixes seem simple. The first fix that needed to be done was a very simple change to some wording which required me to change a cgi file so I made the change tested it and it all worked on my dev system so I then uploaded the file over FTP to the server and nothing worked. So after lots of debugging trying all sorts of things, I remembered something about the binary and ASCII transfer settings for FTP. Mine was set to binary so I switched it over to ASCII uploaded the file again and sure enough everything worked again.

So as a word of advice when working with CGI and FTP always check your transfer settings, had I done this when I started I wouldn’t have wasted hours of my night.
