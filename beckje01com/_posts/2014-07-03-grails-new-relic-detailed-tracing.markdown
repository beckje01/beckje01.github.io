---
author: Jeff Beck
title: "Grails New Relic Detailed Tracing"
date: 2014-07-03 09:36:21 -0500
comments: true
categories:
  - Grails
---
New Relic with Grails by default will trace most web transactions through the controller but will not trace down into services. While most true work of a request belongs in services or libraries the default tracing leaves something to be desired.

This is easily fixed by adding New Relic annotations to services and libraries.


### BuildConfig.groovy Changes
``` groovy
dependencies {
	compile 'com.newrelic.agent.java:newrelic-api:3.4.2'

}
```

### Service Changes
``` groovy
import com.newrelic.api.agent.Trace

class SubscriptionService {

	@Trace
	def save(Subscription subscription) {
    //Work Here
  }
```

At this point your code is ready to give more detailed transactions, but the agent on the server must also be configured to accept custom tracing. The config option for this is not available from the web so you must update the `newrelic.yml` file. Set `enable_custom_tracing` to `true`.

``` yaml
  #enable_custom_tracing is used to allow @Trace on methods
  enable_custom_tracing: true
```

Now you will get any custom tracing added to your application as well as custom tracing from libraries.
