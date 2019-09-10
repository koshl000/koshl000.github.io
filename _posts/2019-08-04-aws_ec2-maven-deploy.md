---
layout: post
title:  "maven을 이용한 ec2 tomcat9에 배포"
categories: maven
tags: maven,deploy
author: moai
---

### %MAVEN_HOME%/conf/settings.xml설정
```xml
<server>
  <id>aws</id>
  <username>centos</username>
  <privateKey>d:/private/instance.pem</privateKey>
</server>
```
- **\<id>aws\</id>**
    - plugin에서 참조할 id 
- **\<username>centos\</username>**
    - ec2 로그인에 사용되는 id(centos->centos,ubuntu->ubuntu,amazon linux->ec2-user)
- **\<privateKey>d:/private/instance.pem<\/privateKey>**
    - ec2 로그인에 사용되는 key파일  



    
### pom.xml 설정
   
```xml
<plugins>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <encoding>utf-8</encoding>
        <compilerArgument>-Xlint:all</compilerArgument>
        <showWarnings>true</showWarnings>
        <showDeprecation>true</showDeprecation>
    </configuration>
</plugin>
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.0</version>
    <configuration>
        <url>http://ec2-host:port/manager/text</url>
        <path>/path</path>
        <server>aws</server>
        <username>tomcat_users.user</username>
        <password>tomcat_users.password</password>
    </configuration>
</plugin>
</plugins>

```
  
### Command Line :

```
$mvn clean tomcat7:deploy -Dmaven.test.skip=true
```

- **-Dmaven.test.skip=true**  
    -test관련 라이프사이클 스킵
- **clean tomcat7:deploy**
    -maven을 clean 시킨후 tocat플러그인 실행

