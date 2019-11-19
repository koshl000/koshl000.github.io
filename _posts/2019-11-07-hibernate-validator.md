---
layout: post
title:  "Hibernate Validator"
categories: Spring
tags: Spring,Validator
author: moai
---

###Dependency
```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.10.Final</version>
</dependency>

<dependency>
    <groupId>org.glassfish.web</groupId>
    <artifactId>javax.el</artifactId>
    <version>2.2.6</version>
</dependency>
```
###Configuration
```xml
<bean id="validationMessageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
        <property name="basename" value="validationMessages" />
        <property name="defaultEncoding" value="utf-8"/>
</bean>
```
또는 messageSource와 validationMessage를 같이 사용할 경우,
```xml
<bean id="messages" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
        <property name="basenames" value="classpath:messages.message,classpath:validationMessages"/>
        <property name="useCodeAsDefaultMessage" value="true"/>
        <property name="defaultEncoding" value="utf-8"/>
</bean>
```
basename의 value값은 validationMessages 혹은 ValidationMessages로만 사용할 것. 다른 값 지정시 properties파일을 찾지 못함. 

