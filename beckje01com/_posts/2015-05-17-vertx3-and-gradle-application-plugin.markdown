---
title: Vertx3 and Gradle Application Plugin
date: 2015-05-17 16:45:08.000000000 -05:00
comments: true
published: true
categories:
- Vertx3
- Gradle
---

Vertx 3 is a great step forward you can now work with Vertx without having to install a command line tool to run it. Now you can use standard Maven or Gradle to run your application.

With Gradle the samples given all are based on the use of the great [Shadow Plugin](https://github.com/johnrengelman/shadow). While this is good for deployments and most local development I missed a few of the features of the application plugin.

Set up your `build.gradle` with the following:

```groovy
//...

apply plugin: 'java'
apply plugin: 'application'
apply plugin: 'com.github.johnrengelman.shadow'

//...

dependencies {
	compile "io.vertx:vertx-core:${project.vertxVersion}"
	compile "io.vertx:vertx-apex:${project.vertxVersion}"
	compile "io.vertx:vertx-auth-service:${project.vertxVersion}"
}

def mainVerticle = "groovy:ex.main.RestVerticle"

mainClassName = "io.vertx.core.Starter"
run {
	args = ["run", mainVerticle]
}

jar {
	manifest {
		attributes 'Main-Verticle': mainVerticle
	}
}

shadowJar {
	classifier = 'fat'

	mergeServiceFiles {
		include 'META-INF/services/io.vertx.core.spi.VerticleFactory'
	}

	dependencies {
		exclude(dependency('io.vertx:codegen'))
		exclude(dependency('junit:junit'))
		exclude(dependency('org.mvel:mvel2'))
		exclude(dependency('log4j:log4j'))
	}
}

```

In this case we are deploying Vertx with verticles. So we set the main verticle which does our setup. I define that as variable since we will need to use it in two places once as an argument to the Vertx starter and again in the Jar manifest. You will note the main verticle includes the type in our case it is Groovy but if you are doing JavaScript you would change it there.

Now we can take advantage of the application plugin allowing us to use the simple `gradle run` instead of depending on the Shadow plugin it is also faster then rebuilding the whole fat jar.

But the main reason I wanted to use the application plugin was the ability to debug easily.

```
$ gradle run --debug-jvm
```

That command will allow us to attach a debugger to 5005 _default_. With these changes Vertx 3 development fits in along side my other Grails 3 and Java 8 projects easily.
