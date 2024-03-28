---
author: Jeff
title: Connector/J 5.1.6 vs Connector/J 5.1.7
excerpt:
layout: post
categories:
  - Java
  - Web Development
tags:
  - Connector/J
  - Java
  - JDBC
  - MySQL
post_format: [ ]
comments: true
date: 2009-04-09T16:19:46+0000
---
While deploying a Java based auto import system I ran into a very odd problem. The code was developed and tested just fine with Connector/J 5.1.6 then while pushing to prod there seemed to be a major issue, the import would run download everything parse it and then clear out the old data but then get stuck. I would then get a series of errors java.sql.SQLException: !Statement.GeneratedKeysNotRequested! well it turned out that the prod sever had Connector/J 5.1.7 installed instead of the 5.1.6 which test had installed. This turned out to be allowing for some wrong practices when it came to prepared statements. It was a pretty easy fix yet it took a bit of time to make sure it happened every where. So why there was a change and what happened is well discussed else where and the links to those will follow. But for now here the change that needs to happen.


``` java
PreparedStatement postinsert = connection.prepareStatement(sql);
```

Just changes to the following. But make sure you do the change for all prepared statements that will use generated keys, its easy to miss one and have the whole app fail because of it.

``` java
PreparedStatement postinsert = connection.prepareStatement(sql,PreparedStatement.RETURN_GENERATED_KEYS);
```

So if you would like to read more about whats going on check out these resources:

*   [The bug report that caused the change][1]
*   [A bug report that calls the change a bug][2]
*   [A bug in the current implementation][3]

 [1]: http://bugs.mysql.com/bug.php?id=34185
 [2]: http://bugs.mysql.com/bug.php?id=41448
 [3]: http://groups.google.com/group/persevere-framework/browse_thread/thread/8c9ea080a1bf9e65
