---
author: Jeff Beck
title: "Grails 3.1.9 Functional Test Port"
date: 2016-07-28 14:31:24 -0500
comments: true
categories:
- Groovy
- Grails
---

With Grails 3.1.9 and above we now get a default port for Functional tests that is randomly assigned. This is a great feature that was added in with this [commit](https://github.com/grails/grails-core/commit/0532f0cef10990c651c01300007458cba9efff86).

While this is a nice addition our tests which were not using Geb were left broken this is due to the fact we now need to know what port was selected for a given run of the tests. So our old test:

<!--more-->

```groovy
@Integration
class UserEndpointSpec extends Specification {  
  def "POST new user"() {
    given:
    ResponseEntity<Map> resp
    RestTemplate restTemplate = new RestTemplate()
    def requestBody = [
        email: 'phil@aol.com',
        fullName: 'Phil',
        password: 'password',
        username: 'phil'
    ]

    when:
    resp = restTemplate.postForEntity("http://localhost:8080/users", requestBody, Map)

    then:
    resp.statusCode == HttpStatus.NO_CONTENT
  }
}
```

Was broken since the app no longer started on 8080, but we can fix that with a new property that the `@Integration` annotation adds which is `serverPort` so the test becomes:

```groovy
@Integration
class UserEndpointSpec extends Specification {  
  def "POST new user"() {
    given:
    ResponseEntity<Map> resp
    RestTemplate restTemplate = new RestTemplate()
    def requestBody = [
        email: 'phil@aol.com',
        fullName: 'Phil',
        password: 'password',
        username: 'phil'
    ]

    when:
    resp = restTemplate.postForEntity("http://localhost:${serverPort}/users", requestBody, Map)

    then:
    resp.statusCode == HttpStatus.NO_CONTENT
  }
}
```

If you are wondering how `serverPort` is actually set you can see it happening inside [IntegrationTestMixinTransformation](https://github.com/grails/grails-core/blob/bf2b027b59890446844756ca3d3124d3d033ea8e/grails-plugin-testing/src/main/groovy/org/grails/compiler/injection/test/IntegrationTestMixinTransformation.groovy#L138). It will add a property to the test class with the port.
