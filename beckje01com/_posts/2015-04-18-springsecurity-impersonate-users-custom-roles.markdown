---
title: SpringSecurity Impersonate Users Custom Roles
date: 2015-04-18 20:54
comments: true
published: true
categories:
- Groovy
- Grails
- SpringSecurity
---
The Grails SpringSecurity plugin has the ability to allow user impersonation which is a really great tool for support. But many times it makes sense to allow your support user to see different things than a user. We use a special role to achieve this behavior we can also have different roles for tiers of support.

By default even impersonation or user switching is turned off in the plugin by default. You can easily turn it on with a config flag:

```
grails.plugin.springsecurity.useSwitchUserFilter = true
```

Spring Security by default will add a new role of [ROLE_PREVIOUS_ADMINISTRATOR](http://docs.spring.io/autorepo/docs/spring-security/3.2.1.RELEASE/apidocs/constant-values.html#org.springframework.security.web.authentication.switchuser.SwitchUserFilter.ROLE_PREVIOUS_ADMINISTRATOR) for the user while impersonating the other user. This will work in most cases but for us it was hard to reason about since we had an idea of admin which was separate from our support team who would mostly be using this tool.

So for our use we wanted our own roles. The ability to do this is exposed in the SwitchUserFilter calling [SwitchUserAuthorityChanger](http://docs.spring.io/autorepo/docs/spring-security/3.2.1.RELEASE/apidocs/org/springframework/security/web/authentication/switchuser/SwitchUserAuthorityChanger.html). With an implementation of the interface you can add any roles you would like.

Example authority changer:

```groovy
class RoleImpersonatedSwitchUserAuthorityChanger implements SwitchUserAuthorityChanger {

	@Override
	public Collection<GrantedAuthority> modifyGrantedAuthorities(UserDetails targetUser, Authentication currentAuthentication, Collection<GrantedAuthority> authoritiesToBeGranted) {
		SwitchUserGrantedAuthority roleImpersonatedSwitchAuthority = new SwitchUserGrantedAuthority("ROLE_IMPERSONATED_USER", currentAuthentication);

		Collection<? extends GrantedAuthority> augmentedAuthoritiesToBeGranted = authoritiesToBeGranted + roleImpersonatedSwitchAuthority

		return augmentedAuthoritiesToBeGranted;
	}
}
```

In Grails we set our authority changer during bootstrap.

### BootStrap.groovy
```groovy 

class BootStrap {

	def switchUserProcessingFilter
	def switchUserAuthorityChanger

	def rabbitTemplate

	def init = { servletContext ->  
    switchUserProcessingFilter.setSwitchUserAuthorityChanger(switchUserAuthorityChanger)

    //....
  }
```

The switch user filter is already exposed as a bean so we can just get that injected and we also exposed the authority changer as a bean so we could just inject it in bootstrap.

There are many other options to think about when allowing impersonation of users, it may even make sense to remove some roles from an impersonated user. That is easily done by simply removing roles from the list retuned from the authority changer.
