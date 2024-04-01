---
author: Jeff Beck
title: >
  Grails Custom UserDetailsService using a
  Grails Service
excerpt:
categories:
  - Grails
tags:
  - grails
  - saml
post_format: [ ]
comments: true
date: 2013-02-04T14:27:27+0000
---
Using the Grails Spring Security Core Plugin I found the need to customize the UserDetailsService and use a Grails service. (Part of the roles logic depended on an external API that we already had a service for.) This was easy to accomplish by subclassing the UserDetailsService class I wanted as a base in my case it was actually the SpringSamlUserDetailsService class because I was using the SAML plugin but normally you would subclass GormUserDetailsService. A great starting example is given in the documentation [here][1].

The difference in my case was the need to use the Grails service, I went with providing the service in the resources.groovy file. Below is the example file of what I used.

**My *resources.groovy***

``` groovy
    import com.example.saml.CustomUserDetailsService
    import org.codehaus.groovy.grails.plugins.springsecurity.SpringSecurityUtils

    beans = {
       	userDetailsService(CustomUserDetailsService) {
    		 grailsApplication = ref('grailsApplication')
    		 myService = ref('myService')  //Here we give the reference to the service we want available.
    		 authorityClassName = SpringSecurityUtils.securityConfig.authority.className
    		 authorityJoinClassName = SpringSecurityUtils.securityConfig.userLookup.authorityJoinClassName
    		 authorityNameField = SpringSecurityUtils.securityConfig.authority.nameField
    		 samlAutoCreateActive = SpringSecurityUtils.securityConfig.saml.autoCreate.active
    		 samlAutoAssignAuthorities = SpringSecurityUtils.securityConfig.saml.autoCreate.assignAuthorities as Boolean
    		 samlAutoCreateKey = SpringSecurityUtils.securityConfig.saml.autoCreate.key as String
    		 samlUserAttributeMappings = SpringSecurityUtils.securityConfig.saml.userAttributeMappings
    		 samlUserGroupAttribute = SpringSecurityUtils.securityConfig.saml.userGroupAttribute as String
    		 samlUserGroupToRoleMapping = SpringSecurityUtils.securityConfig.saml.userGroupToRoleMapping
    		 userDomainClassName = SpringSecurityUtils.securityConfig.userLookup.userDomainClassName
    		 authoritiesPropertyName = SpringSecurityUtils.securityConfig.userLookup.authoritiesPropertyName
    	 }
    }
```

**Snip from *CustomUserDetailsService.groovy***


``` groovy
    class CustomUserDetailsService extends SpringSamlUserDetailsService {
    	def myService
    ...
    }
```


 [1]: http://grails-plugins.github.com/grails-spring-security-core/docs/manual/guide/11%20Custom%20UserDetailsService.html
