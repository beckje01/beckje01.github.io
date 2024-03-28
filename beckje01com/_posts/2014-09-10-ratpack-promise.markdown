---
title: "Ratpack Promise"
date: 2014-09-10 15:33:46 -0500
comments: true
categories:
- Ratpack
- Groovy
---
Ratpack has a core class that is the center of the great asynchronous support, [Promise](http://www.ratpack.io/manual/current/api/index.html?ratpack/exec/Promise.html).

Ratpack Promises are very easy to work with, there are just a few key points:

 * Only attach to a promise one time
 * If dealing with the error case it must be done before the success case
 * They are Lazy
 * In Groovy we depend on [Implicit Closure Coercion](http://mrhaki.blogspot.com/2013/11/groovy-goodness-implicit-closure.html) to change our closures to an Action.

## Happy Path

``` groovy Consuming Value from Promise
Promise promise = somethingThatReturnsPromise()

promise.then {
  println it
}
```

What we are doing here is giving a closure to the promise that once the value is ready the closure will be called with the value passed in as a parameter. We can also be very explicit in what we are getting back from the promise.

``` groovy Explicit Value from Promise
def p = httpClient.get {
  it.url.set(new URI("http://example.com"))
}

p.then { ReceivedResponse receivedResponse ->
  println receivedResponse.statusCode
}
```
If some error occurs while trying to get the value for the `then` block the exception will be thrown. Which can be picked up by some error handler down the chain.

## Error Callback

So for this works great when dealing with the happy path and wanting exceptions. But we also may want to deal with failures to fulfill the promise. So to do this we start with `onError` instead of `then`.

``` groovy Ratpack Promise with Failure Path
httpClient.get {
    it.url.set(new URI("http://example.com"))
} onError {
    println "Something when wrong: ${it.message}"
} then {
    render "Got a ${it.statusCode} status with body of:\n\n${it.body.text}"
}
```
`onError` will pass in a throwable to the closure that you can log or do whatever work you would like in the case of a failure.

## Lazy Promises

Ratpack promises won't actually try to generate the value until the `then` block is called at the end of the current execution. This is done to allow for deterministic asynchronous operations.

``` groovy Deterministic Promise
def doWork() {
  httpClient.get { … }.then { … }
  sleep 5000
  throw new RuntimeException("bang!")
}
```

What will happen in Ratpack is we will always get the exception "bang!", because the get request will not even get started until the `doWork` block of execution is finished. Once finished having a `then{}` will trigger a background thread to start generating the value.

## What not to do

You shouldn't try to attach more than once to a Promise, as what ends up happening is two different promise instances will execute in the background and what we want is only to deal with that value once. So don't do the following:

``` groovy Don't do this

def p = httpClient.get {
  it.url.set(new URI("http://example.com"))
}

p.onError {
  println it
}

p.then {
  println it.statusCode
}

```
Starting in Ratpack 0.9.9 the above code should actually throw an error.
