---
title: Grails 3 and JaCoCo
date: 2015-07-12 21:29
comments: true
published: true
categories:
- Groovy
- Grails
---

Now that we have Gradle as our build system we have a whole range of plugins we can use directly in Gradle. For code coverage I am using the JaCoCo plugin. To use it with Grails we just apply the plugin to the build. By default you will get a HTML report, in the `build/report/jacoco` directory.

```groovy
apply plugin: "jacoco"
```

But Grails 3 has it's test phases split up, out of the box you will have `test` and `integrationTest`. Starting with the JaCoCo gradle plugin, you will get coverage for just your `test` phase.

We can easily fix this by letting JaCoCo know about the phases we want reported on so we just adjust the config for the jacocoTestReport task.

```groovy
jacocoTestReport {
	executionData integrationTest, test
	//...
}
```
