---
title: "Failing @Grab Fixes"
date: 2014-07-25 06:54:30 -0500
comments: true
categories:
 - Groovy
---

If you are working on a Groovy script with @Grab, you will sometimes get download failures for dependencies. Such as the following:

```
General error during conversion: Error grabbing Grapes -- [download failed: com.google.guava#guava;16.0!guava.jar(bundle), download failed: org.javassist#javassist;3.18.1-GA!javassist.jar(bundle)]
```

This issues may have nothing to do with the actual dependency but an issue in your local m2 cache. The quick answer is to just delete `~/.groovy/grapes` and `~/.m2/repository`. But doing this will force you to re-download dependencies.

To only delete the cache for items giving you an issue you just need to delete the correct directories in both m2 and grapes cache. So for our Guava example you would do the following:

```
rm -r ~/.groovy/grapes/com.google.guava
rm -r ~/.m2/repository/com/google/guava

```

After that you should be able to run the groovy script normally.
