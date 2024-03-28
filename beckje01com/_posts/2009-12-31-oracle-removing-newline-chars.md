---
author: Jeff Beck
title: Oracle Removing Newline Chars
excerpt:
categories:
  - Software Engineering
tags:
  - DataBase
  - Oracle
post_format: [ ]
comments: true
date: 2009-12-31T12:32:00+0000
---
We were trying to trim white space from a column that had some newlines. The problem was the oracle trim() was only removing spaces. We found that we had to do two Replace operations Replace(col,char(10),”) and Replace(col,char(13),”) one for linefeed and one for newline. I switch between many Languages and I found it annoying I couldn’t use \n or \r but all well I don’t use Oracle that often.
