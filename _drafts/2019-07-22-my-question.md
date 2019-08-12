---
layout: post
title:  "Jquery load/ready 차이점"
categories: Jquery
tags: Jquery,HTML,DOM
author: moai
---

ec2 여러 서버 중단시 재가동 방법?
다시 시작 후 각각의 서버마다 was를 재 실행 해야되나?

centos7 톰캣 웹페이지 열리지 않을시

1. # yum install tomcat tomcat-admin-webapps tomcat-webapps

2. ps -ef | grep tomcat
tomcat이 재대로 올라 갔는지 부터 확인해보자. 
재대로 올렸다고 생각하지만 에러가 발생하여 올라가다가 죽는 경우도 종종 발생한다.
./start.sh 또는 ./catalina.sh start 명령어를 입력 후에도 톰캣이 올라가지 않는경우 logs/catalina.out을 tail로 확인 후 대처 하자

3.. Port 개방
한 서버에 여러개의 톰캣을 올리기 위해 포트를 변경해서 사용하는 경우가 있다. 이 경우 내가 적용한 포트가 방화벽 정책에 허용되는지 확인해보자.
netstat -ano|grep port 를 이용해서도 포트가 열려있는지 확인할 수 있다.