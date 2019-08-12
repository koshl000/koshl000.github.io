---
layout: post
title:  "[Centos7]Tomcat9 설치"
categories: Linux
tags: Centos,Tomcat
author: moai
---

출처 : [https://blog.boxcorea.com/wp/archives/2405][출처]  

CentOS 7.2 에서 yum으로 tomcat을 설치하면 tomcat 7 이 설치된다. 현재 tomcat은 9버전 이 최신 버전이고 이를 설치하려면 아래와 같이 진행하면 된다.

1. tomcat 다운로드

```bash
# curl -O http://mirror.apache-kr.org/tomcat/tomcat-9/v9.0.13/bin/apache-tomcat-9.0.13.tar.gz
```
 
2. 다운로드 받은 화일의 압축을 풀고 /opt 디렉토리로 이동한다.(/opt/tomcat에 설치)

```bash
# tar xvzpf apache-tomcat-9.0.13.tar.gz
# mv apache-tomcat-9.0.13 /opt
# cd /opt; mv apache-tomcat-9.0.13 tomcat
```




3. tomcat user와 group 생성하고, tomcat 화일의 소유권 변경한다.

```bash
# useradd tomcat
# groupadd tomcat
# cd /opt
# chown -R tomcat:tomcat tomcat
```

4. systemd가 tomcat데몬을 제어하도록 등록한다. 등록하지 않으면, /opt/tomcat/bin/ 디렉토리에서 스크립트를 이용하여 데몬을 실행, 정지해야 한다.
/etc/systemd/system/tomcat.service 화일을 만들고 아래 내용 등록한다.

```bash
# cat /etc/systemd/system/tomcat.service
# Systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target
 
[Service]
Type=forking
 
Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
 
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID
 
User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always
 
[Install]
WantedBy=multi-user.target
```
아래 명령어로 tomcat을 systemd 에 등록하고, tomcat을 실행해 본다.

```bash
# systemctl daemon-reload
# systemctl start tomcat
```

5. 웹 브라우저에서 http://x.x.x.x:8080 으로 접속해 본다(x.x.x.x 는 tomcat이 설치된 서버)
페이지에 접속은 잘 되지만, manager와 host-manager, server status는 접속되지 않는다. 이것은, localhost에서만 접속되도록 설정이 되어 있기 때문이다. 아래 화일에서 관련 부분을 아래처럼 주석처리한다.

/opt/tomcat/webapps/host-manager/META-INF/context.xml 수정(주석처리)
```xml
<Context antiResourceLocking="false" privileged="true" >
<!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
이제, 아래 role과 user를 등록해주면 manager, host manager에 접속이 가능해 진다.

/opt/tomcat/conf/tomcat-users.xml 수정
```xml
  <role rolename="tomcat"/>
  <role rolename="admin-gui"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-status"/>
  <user username="admin" password="passwd" roles="manager-gui,admin-gui,manager-status" />
</tomcat-users>
```

[출처]: https://blog.boxcorea.com/wp/archives/2405