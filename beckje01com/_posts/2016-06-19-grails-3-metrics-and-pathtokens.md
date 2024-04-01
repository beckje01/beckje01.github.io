---
title: Grails 3 Metrics and Path Variables
date: 2016-06-19 16:10
comments: true
published: true
categories:
- Groovy
- Grails
---

Grails 3 is built on top of SpringBoot on of the key components of SpringBoot is [Actuator](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator). Actuator sets up Metrics and healthchecks all the things modern services are expected to just have. Grails 3 ships with the `/metrics` endpoint turned on meaning every request gets tracked by default the endpoint will be available to all logged in users assuming you are using some security.

Most of the time this wouldn't be an issue and would be very nice to have those metrics the problem comes to the fact that currently all requests with Path Variables are treated as different endpoints. So requests to `/items/$id` of `/items/123` and `/items/333` will end up listed as two different metrics with the value of the path variable for everyone to see. This exposes more data than intended and doesn't provide really useful metrics. Its due to an issue in Actuator [GH Issue 5875](https://github.com/spring-projects/spring-boot/issues/5875). So until this is fixed I recommend turning off metrics collection all together for Grails 3.

You can do that with the following config in your `application.yml`

```yaml
endpoints:
  metrics:
    enabled: false
```

I just keep the metrics endpoint off since the health-checks are useful and something we depend on others may want to just shut of Actuator fully which can be done with:

```yaml
endpoints:
   enabled: false
```

Thanks to [Ryan Vanderwerf](https://twitter.com/RyanVanderwerf) for help on this issue.
