---
title: "Grails 3 and Spring Java Config"
date: 2015-04-24 22:52:53 -0400
comments: true
categories:
- Groovy
- Grails
---

The Spring Java config is a great way to work with Spring configuration. It feels similar to Google Guice, which I personally enjoy. Mr. Haki did a great write up on using it with [Grails 2.4](http://mrhaki.blogspot.com/2014/08/grails-goodness-use-spring-java.html).

While working with Grails 3 there are a few areas that don't have plugins yet but do have Spring modules we can leverage. So I started out with the same path given for Grails 2.X.

```yaml application.yml
grails:
    profile: web
    codegen:
        defaultPackage: example
    spring:
        bean:
            packages:
                - example.config
```

I found that while the beans were all wired correctly and `@Autowired` all worked correctly, but any mappings with `@RequestMapping` where always returning a 404. Everything looked wired correctly but Grails wasn't letting any requests through.

So I switched over to using `@ComponentScan` then the requests were properly mapped and everything started working.

Using ComponentScan with Grails 3 is very easy:

```groovy Application.groovy
@ComponentScan("example.config")
class Application extends GrailsAutoConfiguration {
  //...
}
```

This will scan the package of `example.config` for any Spring components so your Spring Java configs can all live in this package and easily get picked up.


This post is the blog form of this [SO Question](http://stackoverflow.com/questions/29736850/grails-3-and-spring-requestmapping).
