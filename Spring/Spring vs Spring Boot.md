# Spring vs Spring Boot

## 1. Spring이란?

**Spring Framework**는 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크로, 간단히 **Spring**이라 불림 <br>
또한, 동적인 웹 사이트를 개발하기 위해 여러가지 서비스를 제공

### 1-1. Spring의 특징

- **제어의 역전(IoC : Inversion of Control)** <br>
  사용자의 제어권을 다른 주체에 넘기는 것을 의미 <br>
  <U>클래스 내부의 객체 생성 → 의존성 객체의 메소드 호출</U>이 아닌 <U>스프링에게 제어를 위임하여 스프링이 만든 객체를 주입 → 의존성 객체의 메소드 호출</U> 구조

- **의존성 주입(DI : Dependency Injection)** <br>
  어떤 객체를 사용하는 주체가 객체를 직접 생성하는게 아니라 객체(B)를 외부(Spring)에서 생성해서 사용하려는 주체 객체(A)에 주입시켜주는 방식 <br> > 이렇게 외부에서 직접 생성하여 관리하는 경우에는 객체 간의 의존성이 감소

- **관점 지향 프로그래밍(AOP : Aspect-Oriented Programming)** <br>
  공통으로 사용되는 기능들을 외부 독립된 클래스로 분리하는 방식 > 공통 로직과 핵심 비즈니스 로직을 분리하여 응집도를 높여 개발 가능

### 1-2. Spring Module

![Spring](/Spring/images/spring.png)

Spring MVC 프레임워크를 사용할 때 applicationContext.xml 이외의 다양한 설정을 통해 의존성을 주입

> Spring MVC는 DispatcherServlet, ViewResolver, Interceptort, Handler, View 등으로 구성

<br>

## 2. Spring Boot란?

**Spring Boot**는 **Spring Framework**를 사용하기 위한 설정의 많은 부분을 자동화하여 사용자가 보다 편하게 **Spring**을 사용할 수 있게 하는 라이브러리

### 2-1. Spring Boot의 특징

- **의존성 관리** <br>
  스프링 부트의 경우 <U>'spring-boot-starter'</U>의 의존성이 여러 종류가 있고 각 라이브러리의 기능과 관련해서 자주 사용되고 서로 호환되는 버전의 모듈 조합을 제공

- **자동 설정** <br>
  애플리케이션에 추가된 라이브러리를 실행하는데 필요한 환경 설정을 알아서 찾아줌 <br>
  또, 개발하는데 필요한 의존성을 추가하면 프레임워크가 이를 자동으로 관리

- **내장 WAS(Web Application Server)** <br>
  웹 애플리케이션을 개발할 때 가장 기본이 되는 의존성인 <U>'spring-boot-starter-web'</U>의 경우 <U>Tomcat</U>을 내장 > 필요에 따라서 웹 서버(jetty, Undertow 등)로 대체 가능

- **모니터링** <br>
  <U>Spring Boot Actuator</U>라는 자체 모니터링 도구가 존재

### 2-2. Spring Boot Starter

기존에는 스프링을 사용할 때 버전까지 명시하고 버전에 맞는 설정을 하였지만, 스프링 부트는 버전 관리를 스프링 부트에 의해서 관리 <br>
따라서 아래처럼 <U>'spring-boot-start-web'</U>을 사용하면 종속된 모든 라이브러리를 알맞게 찾아서 함께 가져오기 때문에 종속성이나 호환 버전에 대해 신경 쓸 필요가 없음

```java
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

<br>

## 3. Spring vs Spring Boot

1. **Spring Boot**는 내장 서버인 **Embedded Tomcat**을 사용하기 때문에 따로 **Tomcat**을 설치하거나 매번 버전 관리를 할 필요가 없음

2. **Spring Boot**는 <U>'spring-boot-starter'</U>를 통한 dependency 자동화가 이뤄짐

   - Spring Framework에서는 각각의 dependency들이 호환되는 버전을 일일히 맞춰야 했기 때문에 하나의 버전을 올리기 위해 다른 dependency까지 바꿔야 한다는 버전 관리의 어려움 존재

   - 하지만 Spring Boot에서는 <U>'spring-boot-starter'</U>를 통해 대부분의 dependency를 관리할 수 있기 때문에 버전 관리가 편리

3. XML 설정이 필요 없음

4. jar 파일을 이용해 자바 옵션만으로 손쉽게 배포가 가능

5. **Spring Boot Actuator**를 이용한 애플리케이션의 모니터링과 관리를 제공

<br>

### 참고 자료

https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/html/overview.html

https://velog.io/@thsruddl77/CS-Spring-Framework-Spring-Boot

https://yummy0102.tistory.com/438

#### Spring vs Spring Boot 면접 질문 관련 참고 사이트

https://yeo-computerclass.tistory.com/401
