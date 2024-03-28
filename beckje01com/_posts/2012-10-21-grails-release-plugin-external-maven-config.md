---
author: Jeff Beck
title: >
  Grails Release Plugin External Maven
  Config
excerpt:
categories:
  - Grails
  - Software Engineering
  - Web Development
tags:
  - grails
  - Maven
  - Web Development
post_format: [ ]
date: 2012-10-21T14:00:18+0000
---
The [grails release plugin][1] is great for publishing plugins to grails central but it will also publish a plugin to a maven repo. You simple need to configure the new maven repo inside build config. But if you want to release snapshots to an internal maven but not have that config checked in publicly that is a bit more tricky. With a quick bit of code added to BuildConfig.groovy we can have it pull in the config from a file we can choose to not checkin. Now with our public code we don’t need to expose our internal login info. See the file changes below for an example.



Add this to BuildConfig.groovy:

``` groovy
    def mavenConfigFile = new File("${basedir}/grails-app/conf/mavenInfo.groovy")
    if (mavenConfigFile.exists()) {
    	def slurpedMavenInfo = new ConfigSlurper().parse(mavenConfigFile.toURL())
    	slurpedMavenInfo.grails.project.repos.each {k, v ->
    		println "Adding maven info for repo $k"
    		grails.project.repos."$k" = v
    	}
    }
    else {
    	println "No mavenInfo file found."
    }
```

Then you can create a file called mavenInfo.groovy:

``` groovy
    grails.project.repos.internalSnap.url = 'http://internal.com:8080/nexus/content/repositories/internal-snapshots/'
    grails.project.repos.internalSnap.username = 'fakeUser'
    grails.project.repos.internalSnap.password = 'fakePass123'
```

Grab the [gist][2].

 [1]: http://grails.org/plugin/release
 [2]: http://bit.ly/QAyVhR
