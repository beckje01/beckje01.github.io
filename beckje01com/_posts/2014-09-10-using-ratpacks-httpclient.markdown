---
title: Using Ratpack's HttpClient
date: 2014-09-10 13:30
comments: true
published: false
categories:
- Ratpack
- Groovy
---
Ratpack comes with a very powerful HTTP Client built into the core. The client is based on Netty and is fully asynchronous and non-blocking.

The basic get request:
``` groovy
httpClient.get {
    it.url.set(new URI("http://example.gas"))
} onError {
    render "Http Request Failed"
} then {
    render "Got a ${it.statusCode} status with body of:\n\n${it.body.text}"
}
```

<!-- more -->

We are using the syntactic sugar method of `httpClient.get` to specify which HTTP method to use, it will return a [Ratpack Promise](/blog/2014/09/10/ratpack-promise/). 
