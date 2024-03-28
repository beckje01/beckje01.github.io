---
title: Grails Integration Testing of Complex Transactions
date: 2015-02-14 15:41
comments: true
published: true
categories:
- Groovy
- Grails
---

Integration testing complex transactions in grails can be tricky due to the default behavior of wrapping integration tests in transactions and rolling them back when complete.

The simple solution is to simply turn off transactions for integration tests, that solution will work but tends to lead to data pollution in downstream tests. Burt Beckwith has a solution in his post [An Alternative Approach for Grails Integration Tests](http://burtbeckwith.com/blog/?p=82). Using this solution we can rebuild the database for each test.

In general most of the time you can use the default transactional testing behavior, therefore we want to only use this method some of the time. To achieve this effect we will modify Burt's original solution slightly.

 * Rebuild the database _after_ each test
 * Depend on the configured data source
 * Update to Spock

## Updated Solution

```groovy NonTransactionalIntegrationSpec.groovy
import org.codehaus.groovy.grails.orm.hibernate.cfg.DefaultGrailsDomainConfiguration
import org.hibernate.SessionFactory
import org.hibernate.cfg.Configuration
import org.hibernate.tool.hbm2ddl.SchemaExport

class NonTransactionalIntegrationSpec extends company.IntegrationSpec {

    @Shared
    private static Configuration _configuration

    @Shared
    def grailsApplication

    static transactional = false

    def setupSpec() {
        if (!_configuration) {
            // 1-time creation of the configuration
            Properties properties = new Properties()
            properties.setProperty 'hibernate.connection.driver_class', grailsApplication.config.dataSource.driverClassName
            properties.setProperty 'hibernate.connection.username', grailsApplication.config.dataSource.username
            properties.setProperty 'hibernate.connection.password', grailsApplication.config.dataSource.password
            properties.setProperty 'hibernate.connection.url', grailsApplication.config.dataSource.url
            properties.setProperty 'hibernate.dialect', 'org.hibernate.dialect.H2Dialect'

            _configuration = new DefaultGrailsDomainConfiguration(grailsApplication: grailsApplication, properties: properties)
        }
    }

    def cleanupSpec() {
      //After spec nuke and pave the test db
      new SchemaExport(_configuration).create(false, true)

      //Clear the sessions
      SessionFactory sf = grailsApplication.getMainContext().getBean('sessionFactory')
      sf.getCurrentSession().clear()
    }

}
```
You'll notice we extend a company.IntegrationSpec there isn't anything special in that just a base integration spec class that can hold our shared testing code such as bootstrapping methods and security methods. Since our data bootstrap logic is set up there we can share the same initial setup across both our normal integration test and our new non-transactional integration specs.


## When to Use

The main time we started to need this new testing method was when doing custom propagation of hibernate transactions.

```groovy
  @Transactional(readOnly = true, propagation = Propagation.REQUIRES_NEW)
  def getSomething(){
    //Do some read only operation.
  }
```

Since this will require a new transaction we will not be able to see the inserts that have happened in the default transaction. We do many of our reads as read only and force a new transaction when we are more read heavy during an operation.

In general anytime you are working directly with transaction rollbacks or propagation it best to test those things directly without having Grails inject an extra transaction in there for you.
