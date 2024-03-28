---
title: Memcached Tomcat Sessions and Grails App Info Plugin
date: 2015-01-14 20:46
comments: true
published: true
categories:
- Groovy
- Grails
- Memcached
---

The [App Info Plugin](http://grails.org/plugin/app-info) for Grails is a great tool. We use it in most of our Grails deployments. One of the great features offered is the ability to view all current sessions and their details. Another tool we are using is [Memcached Session Manager](https://code.google.com/p/memcached-session-manager/) which allows shared sessions between our Tomcat servers, great for rolling deploys and high availability.

We found an issue while running both of these in production. Over time the number of sessions reported by App Info would grow extremely large. It turns out that having the App Info plugin track sessions for you when the sessions are shared via memcached can lead to the plugin holding onto references to sessions that have been destroyed on other severs.

So at this time we can't use the App Info plugin session tracking with the Memcached Session Manager, since it leads to a memory leak as all those sessions have references inside the plugin and can never be garbage collected.

We can simply turn off the session tracking with the following code:

```groovy
grails.plugins.appinfo.useContextListener = false
```

But note this config change effect will effect how a war is made so you can't simply override this in production you will need a new war built.

Below is our memory graph before and after the change:

![Graph showing memory usage before and after the config change.](/images/memchange.png)
