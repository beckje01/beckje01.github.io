---
author: Jeff Beck
title: SAML Matching Endpoints with Tomcat
excerpt:
categories:
  - Grails
  - Java
tags:
  - grails
  - Java
  - opensaml
  - saml
post_format: [ ]
comments: true
date: 2013-02-03T17:09:23+0000
---
Getting, SAML message intended destination endpoint did not match recipient endpoint, errors mean the server itself dosen’t match the urls being given in the SAML messages.

We are using the Grails Spring Security SAML Plugin on a Tomcat server. In my case this was happening because we were doing SSL offloading on the load balancer. So if you look at the logs there should be an error log with the intended destination and the recipient endpoint.

In my case the first error was only different by http vs https. The fix for that was simply to apply the scheme attribute to that connector in tomcat. At which point everything was matching except that the port was now being added as 80 in my endpoint and that wasn’t in the intended endpoint. The fix for this was just to add the proxyPort to the connector as well.

So to fully support the OpenSAML on tomcat with SSL offloading I configured the connector as seen below. Take note of the scheme and proxyPort being set.

``` xml
    <Connector port="8080" protocol="HTTP/1.1"
                   enableLookups="false"
                   maxThreads="250"
                   connectionTimeout="20000"
                   scheme="https"  
                   proxyPort="443"/>
```
<!-- more -->

```
    3377265 2013-02-01 11:31:30,997 ERROR [http-8080-9] decoding.BaseSAMLMessageDecoder.checkEndpointURI (BaseSAMLMessageDecoder.java:215) - SAML message intended destination endpoint 'https://example.com/app/saml/SSO/alias/https://example.com' did not match the recipient endpoint 'https://example.com:80/app/saml/SSO/alias/https://example.com'
    Feb 1, 2013 11:31:30 AM org.apache.catalina.core.StandardWrapperValve invoke
    SEVERE: Servlet.service() for servlet default threw exception
    org.opensaml.common.SAMLRuntimeException: Incoming SAML message is invalid
            at org.springframework.security.saml.SAMLProcessingFilter.attemptAuthentication(SAMLProcessingFilter.java:93)
            at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:199)
            at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:381)
            at org.springframework.security.saml.metadata.MetadataDisplayFilter.doFilter(MetadataDisplayFilter.java:83)
            at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:381)
            at org.springframework.security.saml.SAMLEntryPoint.doFilter(SAMLEntryPoint.java:102)
            at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:381)
            at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:79)
            at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:381)
            at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:168)
            at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:346)
            at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:259)
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
            at org.codehaus.groovy.grails.web.servlet.mvc.GrailsWebRequestFilter.doFilterInternal(GrailsWebRequestFilter.java:69)
            at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:76)
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
            at org.codehaus.groovy.grails.web.filters.HiddenHttpMethodFilter.doFilterInternal(HiddenHttpMethodFilter.java:65)
            at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:76)
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
            at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:88)
            at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:76)
            at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:346)
            at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:259)
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:235)
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:206)
            at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:233)
            at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:191)
            at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:127)
            at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:102)
            at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:109)
            at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:298)
            at org.apache.coyote.http11.Http11AprProcessor.process(Http11AprProcessor.java:864)
            at org.apache.coyote.http11.Http11AprProtocol$Http11ConnectionHandler.process(Http11AprProtocol.java:579)
            at org.apache.tomcat.util.net.AprEndpoint$Worker.run(AprEndpoint.java:1665)
            at java.lang.Thread.run(Thread.java:662)
    Caused by: org.opensaml.xml.security.SecurityException: SAML message intended destination endpoint did not match recipient endpoint
            at org.opensaml.common.binding.decoding.BaseSAMLMessageDecoder.checkEndpointURI(BaseSAMLMessageDecoder.java:217)
            at org.opensaml.saml2.binding.decoding.BaseSAML2MessageDecoder.decode(BaseSAML2MessageDecoder.java:72)
            at org.springframework.security.saml.processor.SAMLProcessorImpl.retrieveMessage(SAMLProcessorImpl.java:105)
            at org.springframework.security.saml.processor.SAMLProcessorImpl.retrieveMessage(SAMLProcessorImpl.java:172)
            at org.springframework.security.saml.SAMLProcessingFilter.attemptAuthentication(SAMLProcessingFilter.java:77)
            ... 37 more
    Servlet.service() for servlet default threw exception
    org.opensaml.common.SAMLRuntimeException: Incoming SAML message is invalid
            at java.lang.Thread.run(Thread.java:662)
            Caused by: org.opensaml.xml.security.SecurityException: SAML message intended destination endpoint did not match recipient endpoint
                    at org.opensaml.common.binding.decoding.BaseSAMLMessageDecoder.checkEndpointURI(BaseSAMLMessageDecoder.java:217)
                    at org.opensaml.saml2.binding.decoding.BaseSAML2MessageDecoder.decode(BaseSAML2MessageDecoder.java:72)
            ... 1 more

```
