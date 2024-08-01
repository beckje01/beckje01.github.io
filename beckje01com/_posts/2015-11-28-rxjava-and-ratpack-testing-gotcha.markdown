---
title: RxJava and Ratpack Testing Gotcha
date: 2015-11-28 09:48
comments: true
published: true
categories:
- RxJava
- Ratpack
---

Ratpack tends to stay out of the way when building out functionality so there are times you may make classes that use RxJava and Guice without anything from Ratpack. But there is one caveat during testing, if you are using RxJava in a unit test without any Ratpack integration that will work just fine. As soon as there is another test in the same execution that uses Ratpacks integration (`ratpack.dependency("rx")`) with RxJava such as a functional test you will start seeing the following error in the standard out:

```
java.lang.IllegalStateException: Cannot install RxJava integration because another execution hook (class rx.plugins.RxJavaObservableExecutionHookDefault) is already installed
	at ratpack.rx.RxRatpack.initialize(RxRatpack.java:101) ~[ratpack-rx-1.1.0.jar:na]
	at ratpack.rx.RxRatpack$initialize$0.call(Unknown Source) ~[na:na]
	at org.codehaus.groovy.runtime.callsite.CallSiteArray.defaultCall(CallSiteArray.java:48) [groovy-all-2.4.4.jar:2.4.4]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:113) [groovy-all-2.4.4.jar:2.4.4]
	at org.codehaus.groovy.runtime.callsite.AbstractCallSite.call(AbstractCallSite.java:117) [groovy-all-2.4.4.jar:2.4.4]
```

<!--more-->

Many of your functional tests will start failing with very unpredictable results. What is happening is any use of RxJava that runs before the Ratpacks integration will set up an execution hook meaning the strong guarantees about execution Ratpack provides will not be available leading to unpredictable behavior. This is an easy fix for the test, we just need Ratpack to setup the hook in these other tests we can simply add the following to a Spock spec:

```groovy
import ratpack.rx.RxRatpack

def setupSpec() {
  RxRatpack.initialize()
}
```


The same initialize can be used in your favorite testing tools, it just needs to happen before the tests are run once and thats what `setupSpec` gives us in Spock.
