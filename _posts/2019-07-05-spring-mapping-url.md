---
layout: post
title:  "Servlet Mapping /와 /* 차이"
categories: Spring
tags: Spring,tomcat
author: moai
---
${TOMCAT_HOME}/conf/web.xml을 보면
```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
<servlet>
	<servlet-name>default</servlet-name>
	<servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
	...
</servlet>

<!-- The mappings for the JSP servlet -->
<servlet-mapping>
    <servlet-name>jsp</servlet-name>
    <url-pattern>*.jsp</url-pattern>
    <url-pattern>*.jspx</url-pattern>
</servlet-mapping>

<servlet>
	<servlet-name>jsp</servlet-name>
	<servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
	...
</servlet>
```




와같은 맵핑 정보가 있는 것을 볼 수 있는데 *.jsp, *.jspx와 같은 url 패턴은
JspServlet이 처리하고, DefaultServlet은 spring Controller mapping과 jsp 패턴에
걸리지 않는 요청을 처리한다.

```xml
<servlet>
    <servlet-name>springDispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
    <multipart-config/>
  </servlet>
  <servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>
```
하지만 위와같이 DispatcherServlet을 /*로 설정하면 /*은 모든 요청을 처리한다는 의미이기 때문에
DispatcherServlet이 모든 요청을 처리하게 된다. 그렇기 때문에 `${TOMCAT_HOME}/conf/web.xml`의 jsp매핑이 실행되지 않는다.
  
그렇기에 이를 해결하기 위해서는 DispatcherServlet을  `/`로 매핑하면 된다.  
그러나 `/`로 매핑시에는 기존 DefaultServlet의 매핑을 덮어써 정적리소스(.jsp이외)요청이 실패하게 된다.  
해결을 위해서는 servlet-context.xml에 
```xml
<mvc:resources mapping="/resources/**" location="/resources/"/>
```
혹은
```xml
<mvc:default-servlet-handler />
```
을 설정해주면 해결할 수 있다.



                                             
                                             
