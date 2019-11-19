---
layout: post
title:  "Ioc/DI"
categories: Framework
tags: Framework
author: moai
---
<!--
출처 : [https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/][출처]  
-->

#IoC란?
IoC 컨테이너 개념을 이해하기 위하여 이와 같은 컨테이너가 왜 등장하게 되었는지를 먼저 이해하는 것이 중요합니다.
애플리케이션 코드를 작성할 때, 특정 기능이 필요하면 라이브러리 사용하곤 합니다. 이때는 프로그램의 흐름을 제어하는 주체가 애플리케이션 코드입니다. 하지만 프레임워크(Framework) 기반의 개발에서는 프레임워크 자신이 흐름을 제어하는 주체가 되어, 필요 할 때마다 애플리케이션 코드를 호출하여 사용합니다.
프레임워크에서 이 제어권을 가지는 것이 바로 <code>컨테이너(Container)</code>입니다. 객체에 대한 제어권이 개발자로부터 컨테이너에게 넘어가면서 객체의 생성부터 생명주기 관리까지의 모든 것을 컨테이너가 맡아서 하게됩니다. 이를 일반적인 제어권의 흐름이 바뀌었다고 하여 <code>IoC(Inversion of Control : 제어의 역전)</code> 라고 합니다.
먼저 지금까지 일반적으로 개발하던 방식에 대해서 생각해보아야 합니다. 모든 인스턴스에 대한 생성 권한은 지금까지 모든 개발자들에게 있었습니다. 즉, 작성하는 코드상에서 개발자가 직접 생성했다는 것입니다. EJB나 IoC 컨테이너를 사용하지 않았던 개발자들은 지금까지 이와 같은 방식을 사용했습니다.

#사용하는 목적
IoC를 사용하는 목적에 대해서는 지금까지의 클래스호출 방식의 변화를 살펴보면 더 쉽게 이해할 수 있습니다.

#클래스 호출 방식
클래스내에 선언과 구현이 같이 있기 때문에 다양한 형태로 변화가 불가능합니다.
![class1](/assets/images/20191119/class1.png){: .center}  
   
#인터페이스 호출 방식
클래스를 인터페이스와 인터페이스를 상속받아 구현하는 클래스로 분리했습니다. 구현클래스 교체가 용이하여 다양한 변화가 가능합니다. 그러나 구현클래스 교체시 호출클래스의 코드에서 수정이 필요합니다. (부분적으로 종속적)
![class2](/assets/images/20191119/class2.png){: .center}  

#팩토리 호출 방식
팩토리 방식은 팩토리가 구현클래스를 생성하기 때문에 호출클래스는 팩토리를 호출 하는 코드로 충분합니다. 구현클래스 변경시 팩토리만 수정하면 되기 때문에 호출클래스에는 영향을 미치지 않습니다. 그러나 호출클래스에서 팩토리를 호출하는 코드가 들어가야 하는 것 또한 팩토리에 의존함을 의미합니다.
![class3](/assets/images/20191119/class3.png){: .center}   

#IoC
팩토리 패턴의 장점을 더해 어떠한 것에도 의존하지 않는 형태가 되었습니다. 실행시점에 클래스간의 관계가 형성이 됩니다. 즉, 의존성이 삽입된다는 의미로 IoC를 DI라는 표현으로 사용합니다.
![class4](/assets/images/20191119/class4.png){: .center}   

#IoC 용어 정리

- bean : 스프링에서 제어권을 가지고 직접 만들어 관계를 부여하는 오브젝트 Java Bean, EJB의 Bean과 비슷한 오브젝트 단위의 애플리케이션 컴포넌트이다. 하지만 스프링을 사용하는 애플리케이션에서 만들어지는 모든 오브젝트가 빈은 아니다. 스프링의 빈은 스프링 컨테이너가 생성하고 관계설정, 사용을 제어해주는 오브젝트를 말한다.
- bean factory : 스프링의 IoC를 담당하는 핵심 컨테이너 Bean을 등록/생성/조회/반환/관리 한다. 보통 bean factory를 바로 사용하지 않고 이를 확장한 application context를 이용한다. BeanFactory는 bean factory가 구현하는 interface이다. (getBean()등의 메서드가 정의되어 있다.)
- application context : bean factory를 확장한 IoC 컨테이너 Bean의 등록/생성/조회/반환/관리 기능은 bean factory와 같지만, 추가적으로 spring의 각종 부가 서비스를 제공한다. ApplicationContext는 application context가 구현해야 하는 interface이며, BeanFactory를 상속한다.
- configuration metadata : application context 혹은 bean factory가 IoC를 적용하기 위해 사용하는 메타정보 스프링의 설정정보는 컨테이너에 어떤 기능을 세팅하거나 조정하는 경우에도 사용하지만 주로 bean을 생성/구성하는 용도로 사용한다.
- container (ioC container) : IoC 방식으로 bean을 관리한다는 의미에서 bean factory나 application context를 가리킨다. application context는 그 자체로 ApplicationContext 인터페이스를 구현한 오브젝트를 말하기도 하는데, 하나의 애플리케이션에 보통 여러개의 ApplicationContext 객체가 만들어진다. 이를 통칭해서 spring container라고 부를 수 있다.

[출처]: https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container