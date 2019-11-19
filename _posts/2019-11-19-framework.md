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




## 사용하는 목적
IoC를 사용하는 목적에 대해서는 지금까지의 클래스호출 방식의 변화를 살펴보면 더 쉽게 이해할 수 있습니다.

## 클래스 호출 방식
클래스내에 선언과 구현이 같이 있기 때문에 다양한 형태로 변화가 불가능합니다.
![class1](/assets/images/20191119/class1.jpg){: .center}  
   
## 인터페이스 호출 방식
클래스를 인터페이스와 인터페이스를 상속받아 구현하는 클래스로 분리했습니다. 구현클래스 교체가 용이하여 다양한 변화가 가능합니다. 그러나 구현클래스 교체시 호출클래스의 코드에서 수정이 필요합니다. (부분적으로 종속적)
![class2](/assets/images/20191119/class2.jpg){: .center}  

## 팩토리 호출 방식
팩토리 방식은 팩토리가 구현클래스를 생성하기 때문에 호출클래스는 팩토리를 호출 하는 코드로 충분합니다. 구현클래스 변경시 팩토리만 수정하면 되기 때문에 호출클래스에는 영향을 미치지 않습니다. 그러나 호출클래스에서 팩토리를 호출하는 코드가 들어가야 하는 것 또한 팩토리에 의존함을 의미합니다.
![class3](/assets/images/20191119/class3.jpg){: .center}   

## IoC
팩토리 패턴의 장점을 더해 어떠한 것에도 의존하지 않는 형태가 되었습니다. 실행시점에 클래스간의 관계가 형성이 됩니다. 즉, 의존성이 삽입된다는 의미로 IoC를 DI라는 표현으로 사용합니다.
클래스내에 선언과 구현이 같이 있기 때문에 다양한 형태로 변화가 불가능합니다.  
![class1](/assets/images/20191119/class4.jpg){: .center}  

## IoC 용어 정리

- bean : 스프링에서 제어권을 가지고 직접 만들어 관계를 부여하는 오브젝트 Java Bean, EJB의 Bean과 비슷한 오브젝트 단위의 애플리케이션 컴포넌트이다. 하지만 스프링을 사용하는 애플리케이션에서 만들어지는 모든 오브젝트가 빈은 아니다. 스프링의 빈은 스프링 컨테이너가 생성하고 관계설정, 사용을 제어해주는 오브젝트를 말한다.
- bean factory : 스프링의 IoC를 담당하는 핵심 컨테이너 Bean을 등록/생성/조회/반환/관리 한다. 보통 bean factory를 바로 사용하지 않고 이를 확장한 application context를 이용한다. BeanFactory는 bean factory가 구현하는 interface이다. (getBean()등의 메서드가 정의되어 있다.)
- application context : bean factory를 확장한 IoC 컨테이너 Bean의 등록/생성/조회/반환/관리 기능은 bean factory와 같지만, 추가적으로 spring의 각종 부가 서비스를 제공한다. ApplicationContext는 application context가 구현해야 하는 interface이며, BeanFactory를 상속한다.
- configuration metadata : application context 혹은 bean factory가 IoC를 적용하기 위해 사용하는 메타정보 스프링의 설정정보는 컨테이너에 어떤 기능을 세팅하거나 조정하는 경우에도 사용하지만 주로 bean을 생성/구성하는 용도로 사용한다.
- container (ioC container) : IoC 방식으로 bean을 관리한다는 의미에서 bean factory나 application context를 가리킨다. application context는 그 자체로 ApplicationContext 인터페이스를 구현한 오브젝트를 말하기도 하는데, 하나의 애플리케이션에 보통 여러개의 ApplicationContext 객체가 만들어진다. 이를 통칭해서 spring container라고 부를 수 있다.

## DI
IoC는 직관적이지 못하기 때문에 DI(Dependency Injection)라고도 부릅니다. DI는 오브젝트를 생성하고 오브젝트끼리의 관계를 생성해 소프트웨어의 부품화 및 설계를 가능하게 합니다. DI를 이용하면 인터페이스 기반의 컴포넌트를 쉽게 구현할 수 있습니다. DI를 우리말로 옮기면 의존 관계의 주입입니다. 쉽게 말하면 오브젝트 사이의 의존 관계를 만드는 것입니다. 어떤 오브젝트의 프로퍼티(인스턴스 변수)에 오브젝트가 이용할 오브젝트를 설정한다는 의미입니다. 이를 학술적으로 말하면, 어떤 오브젝트가 의존(이용)할 오브젝트를 주입 혹은 인젝션(프로퍼티에 설정)한다는 것입니다. DI를 구현하는 컨테이너는 단순한 인젝션 외에도 클래스의 인스턴스화 등의 생명 주기 관리 기능이 있는 경우가 많습니다.
클래스에서 new 연산자가 사라졌다는 사실이 중요합니다. 클래스에서 new 연산자가 사라짐으로써 개발자가 팩토리 메서드 같은 디자인 패턴을 구사하지 않아도 DI 컨테이너가 건내주는 인스턴스를 인터페이스로 받아서 인터페이스 기반의 컴포넌트화를 구현할 수 있게 됐습니다.

## @Autowired와 @Component
인스턴스 변수 앞에 @Autowired를 붙이면 DI 컨테이너가 그 인스턴스 변수의 형에 대입할 수 있는 클래스를 @Component가 붙은 클래스 중에서 찾아내 그 인스턴스를 인젝션해줍니다(정확히는 Bean 정의에서 클래스를 스캔할 범위를 정해야 합니다). 인스턴스 변수로의 인젝션은 접근 제어자가 private라도 인젝션 할 수 있으므로 Setter 메서드를 만들 필요는 없습니다. (과거에 캡슐화의 정보 은닉에 반하는 것이 아니냐는 논의가 있었지만, 현재는 편리함에 밀려 그런 논의를 보기 힘들어졌습니다.)
만약 @Component가 붙은 클래스가 여러 개 있어도 형이 다르면 @Autowired가 붙은 인스턴스 변수에 인젝션되지 않습니다. 이렇게 형을 보고 인젝션하는 방법을 byType이라고 합니다.
  
![class5](/assets/images/20191119/class5.png){: .center}   
1.위의 사진과 같이 인터페이스에 구현 클래스가 2개여서 @Autowired로 인젝션할 수 있는 클래스의 형이 2개 존재한다면 에러가 발생합니다. 인젝션할 수 있는 클래스의 형은 반드시 하나로 해야합니다. 하지만 이래서는 인터페이스의 구현 클래스를 테스트용 클래스 등 다른 클래스로 바꿀 경우에 불편합니다. 그래서 이를 회피하는 세 가지 방법에 대해 알아보겠습니다.
우선할 디폴트 Bean을 설정하는 @Primary를 @Bean이나 @Component에 부여하는 방법 (Bean 정의 파일에서는 <bean primary="true">)
```java
@Component
@Primary
public class ProductDaoImpl implements ProductDao {
...(생략)...
}
```

2.@Autowired와 병행해서 @Qualifier를 하는 방법 단, 이 경우는 @Component에도 이름을 같이 지정해야 한다. 이렇게 인젝션할 클래스를 형이 아닌 이름으로 찾아주는 방법을 byName이라고 한다. (물론 @Component에 같은 이름이 붙은 클래스가 중복되면 오류가 발생한다.)
```java
@Autowired
@Qualifier("productDao")
private ProductDao productDao;

-----------------------------------------

@Component("productDao")
public class ProductDaoImpl implements ProductDao {
...(생략)...
}
```
3.Bean 정의 파일인 context:component-scan을 이용하는 방법 (context:component-scan을 어느 정도 크기의 컴포넌트마다 기술해두고, 만약 어떤 컴포넌트를 테스트용으로 바꾸고자 할 때는 그 컴포넌트 부분의 정의만 테스트용 부품을 스캔하게 수정하는 방법이다.)

#확장된 @Component
@Component에는 확장된 어노테이션이 있습니다. 웹 애플리케이션 개발에는 @Component를 이용할 것이 아니라 클래스가 어느 레이어에 배치될지 고려해서 배치될 레이어에 있는 @Component 확장 어노테이션을 사용하는 것이 좋습니다. 예를 들어 ProductServiceImpl은 @Component가 아니라 @Service로 바꾸는 편이 좋고, ProductDaoImpl 클래스도 @Component가 아니라 @Repository로 바꾸는 편이 좋습니다.  
>- @Controller : 프레젠테이션 층 스프링 MVC용 어노테이션  
>- @Service : 비즈니스 로직 층 Service용 어노테이션, @Component와 동일
>- @Repository : 데이터 엑세스 층의 DAO용 어노테이션
>- @Configuration : Bean 정의를 자바 프로그램에서 실행하는 JavaConfig용 어노테이션

@Component와 함께 사용하는 어노테이션의 하나로 @Scope가 있습니다. @Scope 뒤에 Value 속성을 지정하면 인스턴스화와 소멸을 제어할 수 있습니다. @Scope를 생략하면 해당 클래스는 싱글턴이 됩니다.
```java
@Component
@Scope("singletone")
public class ProductDaoImple implements ProductDao {
...(생략)...
}
```

>-singleton : 인스턴스를 싱글턴으로 함
>-prototype : 이용할 때마다 인스턴스화함
>-request : Servlet API의 request 스코프인 동안만 인스턴스가 생존함
>-session : Servlet API의 session 스코프인 동안만 인스턴스가 생존함
>-application : Servlet API의 application 스코프인 동안만 인스턴스가 생존함

value 속성의 값은 직접 문자열로 넣어도 되지만, 상수가 존재하기 때문에 상수를 사용하는 것이 좋습니다.
  
>-singleton : BeanDefinition.SCOPE_SINGLETON
>-prototype : BeanDefinition.SCOPE_PROTOTYPE
>-request : WebApplicationContext.SCOPE_REQUEST
>-session : WebApplicationContext.SCOPE_SESSION
>-application : WebApplicationContext.SCOPE_APPLICATION

[출처]: https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container