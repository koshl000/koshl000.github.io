---
layout: post
title:  "SpringSecurity 4.0.x 필터 순서"
categories: Spring Security
tags: Spring Security
author: moai
---

The order that filters are defined in the chain is very important. Irrespective of which filters you are actually using, 
the order should be as follows:

## `ChannelProcessingFilter`
it might need to redirect to a different protocol
## `SecurityContextPersistenceFilter`
`SecurityContext` can be set up in the `SecurityContextHolder` 
at the beginning of a web request, and any changes to the `SecurityContext` can be copied to the `HttpSession`
when the web request ends (ready for use with the next web request)




## `ConcurrentSessionFilter`
It uses the `SecurityContextHolder` functionality and needs to update 
the `SessionRegistry` to reflect ongoing requests from the principal
Authentication processing mechanisms - `UsernamePasswordAuthenticationFilter`, `CasAuthenticationFilter`, `BasicAuthenticationFilter` 
etc - so that the `SecurityContextHolder` can be modified to contain a valid `Authentication` request token

## `SecurityContextHolderAwareRequestFilter`
If you are using it to install a Spring Security aware `HttpServletRequestWrapper` into your servlet container

## `JaasApiIntegrationFilter`
If a `JaasAuthenticationToken` is in the `SecurityContextHolder` this will process 
the `FilterChain` as the Subject in the `JaasAuthenticationToken`

## `RememberMeAuthenticationFilter`
If no earlier authentication processing mechanism updated the `SecurityContextHolder`,
and the request presents a cookie that enables remember-me services to take place, 
a suitable remembered `Authentication` object will be put there

## `AnonymousAuthenticationFilter`
If no earlier authentication processing mechanism updated the `SecurityContextHolder`, an anonymous `Authentication` object will be put there

## `ExceptionTranslationFilter`
Catch any Spring Security exceptions so that either an HTTP error response can be returned or an appropriate `AuthenticationEntryPoint` can be launched

## `FilterSecurityInterceptor`
Protect web URIs and raise exceptions when access is denied