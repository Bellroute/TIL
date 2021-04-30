# Spring Framework & Spring boot

### Spring Framework란?

Java 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크

- **오픈소스(Open Source)** : 소프트웨어 혹은 하드웨어의 제작자의 권리를 지키면서 원시 코드를 누구나 열람할 수 있도록 한 소프트웨어 혹은 오픈 소스 라이선스에 준하는 모든 통칭을 일컫는다. (소스가 공개되어 여러 개발자가 플랫폼을 함께 개발, 구축, 보완해 나가는 시스템. ) => Spring은 오픈 소스의 장점을 취하면서 동시에 오픈 소스 제품의 단점과 한계를 잘 극복함.
- **엔터프라이즈 개발 용이** : 개발자가 복잡하고 실수하기 쉬운 Low Level에 많이 신경 쓰지 않으면서 Business Logic 개발에 전념할 수 있도록 해준다.
- **경량급 프레임워크** : Spring 이전의 프레임워크들은 너무 무겁고, 복잡하고, 특정 분야에 전문적이며, 다른 프레임워크와의 융화가 쉽지 않았음. 스프링 프레임워크는 이러한 문제점을 대부분 극복하였음.
- **애플리케이션 프레임워크** : 특정 계층이나 기술, 업무 분야에 국한되지 않고 애플리케이션의 전 영역을 포괄하는 범용적인 프레임워크로서 기능함.

**`스프링이 다른 프레임워크와 무엇이 다른가?`**

- 하나의 기능을 위해 너무 많은 구조를 필요로 했던 프레임워크와는 달리 스프링은 필요한 특정 기능을 위주로 간단한 jar 파일(Java archive)을 이용하기 때문에 **가볍다**.
- 다른 프레임워크는 객체 간의 관계를 구성하기 위해 별도의 API를 공부해야 했었지만 스프링은 일반적인 Java 문법을 사용하기 때문에 **간단하여** 진입장벽이 낮다.
- 스프링은 웹이나 데이터베이스와 같은 한 분야가 아닌 **전체를 설계하는 용도**로 사용된다.
- 스프링은 전체 구조에 집중하기 때문에 변화에 따른 수정을 최소화 한다. 따라서 **다른 종류의 프레임워크와 공존**하기 쉬운 프레임워크이다.

<br>

### Spring Framework 전략

엔터프라이즈 개발의 복잡함을 상대하는 Spring의 전략은 다음과 같다.

![](https://t1.daumcdn.net/cfile/tistory/99095E485C70D68E32)

#### Portable Service Abstraction(서비스 추상화)

트랜젝션 추상화, ORM 추상화, 데이터 액세스의 Exception 변환 기능 등 <u>기술적인 복잡함은 추상화를 통해 Low Level의 기술 구현 부분과 기술을 사용하는 인터페이스로 분리</u>한다.

#### 객체 지향과 DI(Dependency Injection)

Spring은 <u>객체지향에 충실한 설계가 가능하도록 단순한 객체형태로 개발</u>할 수 있고, DI는 <u>유연하게 확장 가능한 객체를 만들어 두고 그 관계는 외부에서 다이내믹하게 설정</u>해준다.

#### AOP(Aspect Oriented Programming)

AOP는 <u>애플리케이션 로직을 담당하는 코드에 남아있는 기술 관련 코드를 분리해서 별도의 모듈로 관리</u>하게 해주는 강력한 기술이다.

#### POJO(Plain Old Java Object)

POJO는 객체지향 원리에 충실하면서, <u>특정 환경이나 규약에 종속되지 않고 필요에 따라 재활용 될 수 있는 방식으로 설계된 객체</u>이다.

<br>

### Spring Framework 특징

#### 경량 컨테이너 역할

Spring 컨테이너는 Java 객체의 LifeCycle을 관리하며, Spring 컨테이너로부터 필요한 객체를 가져와 사용할 수 있다.

#### IoC(Inversion of Control) 지원

컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서 필요에 따라 스프링에서 사용자의 코드를 호출한다.

#### DI(Dependency Injection) 지원

Spring은 설정파일이나 어노테이션을 통해서 객체 간의 의존관계를 설정할 수 있도록 하고 있다.

#### AOP(Aspect Oriented Programming) 지원

Spring은 트랜젝션이나 로깅, 보안과 같이 공통적으로 필요로 하는 모듈들을 실제 핵심 모듈에서 분리해서 적용 할 수 있다.

#### POJO(Plain Old Java Object) 지원

Spring 컨테이너에 저장되는 Java객체는 일반적인 J2EE 프레임워크에 비해 구현을 위해 특정한 인터페이스를 구현하거나 상속을 받을 필요가 없어 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가볍다.

#### 트랜젝션 처리를 위한 일관된 방법을 지원

JDBC, JTA 등 어떤 트랜젝션을 사용하던 설정을 통해 정보를 관리하므로 트랜젝션 구현에 상관없이 동일한 코드 사용가능

#### 영속성과 관련된 다양한 API 지원

Spring은 Mybatis, Hibernate 등 데이터베이스 처리를 위한 ORM(Object Relational Mapping) 프레임워크들과의 연동 지원

#### 높은 확장성

스프링 프레임워크에 통합하기 위해 간단하게 기존 라이브러리를 감싸는 정도로 스프링에서 사용이 가능하기 때문에 수많은 라이브러리가 이미 스프링에서 지원되고 있고 스프링에서 사용되는 라이브러리를 별도로 분리하기도 용이하다.

<br>

### Spring Framework의 기능요소

![img](https://t1.daumcdn.net/cfile/tistory/9968854E5C70D5B22E)

#### Core 컨테이너

Core 컨테이너는 Spring프레임워크의 기본기능을 제공한다. 이 모듈에 있는 BeanFactory는 Spring의 기본 컨테이너이면서 스프링 DI의 기반이다.

#### Context

Context모듈은 BeanFactory의 개념을 확장한 것으로 국제화(I18N) 메시지, 애플리케이션 생명주기 이벤트, 유효성 검증을 지원한다.

#### DAO

DAO 패키지는 JDBC에 대한 추상화 계층으로 JDBC 코딩이나 예외처리 하는 부분을 간편화 시켰으며, AOP 모듈을 이용해 트랜젝션 관리 서비스도 제공한다.

#### ORM

Mybatis, Hibernate, JPA 등 널리 사용되는 ORM 프레임워크과의 연결고리를 제공한다. ORM 제품들을 Spring의 기능과 조합해서 사용할 수 있도록 해준다.

#### AOP

AOP 모듈을 통해 Aspect 지향 프로그래밍을 지원한다. AOP 모듈은 스프링 애플리케이션에서 Aspect를 개발할 수 있는 기반을 지원한다.

#### Web

일반적인 웹 애플리케이션 개발에 필요한 기본기능을 제공하고, Webwork나 Struts와 같은 다른 웹어플리케이션 프레임워크와의 통합을 지원한다.

#### WebMVC

MVC(Model/View/Controller) 패러다임은 사용자 인터페이스가 애플리케이션 로직과 분리되는 웹 애플리케이션을 만드는 경우에 일반적으로 사용되는 패러다임이다. 이 패러다임을 바탕으로 웹 계층에서 결합도를 낮추는 Spring MVC 프레임워크가 있다.

<br>

### Spring Boot?

Spring Framework는 기능이 많은만큼 환경설정이 복잡한 편이다. 이에 어려움을 느끼는 사용자들을 위해 나온 것이 바로 Spring Boot이다. Spring Boot는 Spring를 사용하기 위한 설정의 많은 부분을 자동화하여 사용자가 편하게 Spring을 활용할 수 있도록 돕는다. (최소한의 설정으로 스프링 플랫폼과 서드파티 라이브러리를 사용할 수 있도록 하고 있다.)

#### Spring Framework vs Spring Boot - 등장 배경 관점

<u>무엇을 해결하고자 등장했는지</u>를 기준으로 비교를 하면 다음과 같다고 볼 수 있다.

**Spring Framework - 개발자들이 애플리케이션을 조금 더 쉽게 구현할 수 있도록 도와주자!**

- Dependency Injection(DI) - 의존성 주입
  - 객체 간 결합을 느슨하게 만듦
  - 코드 재사용성 증가 및 단위테스트가 용이
- 중복된 코드 제거
  - ex. JDBC template
  - 비즈니스 로직에만 집중 가능
- 다른 프레임워크와의 통합
  - ex. JUnit & Mockito for Unit Testing
  - 여러 훌륭하게 제공되는 프레임워크를 통해 해결하고자 하는 문제를 높은 품질로 해결

**Spring Boot - 스프링 프레임워크를 더 쉽게 사용할 수 있도록 도와주자!**

- Auto Configuration 자동 실행
- 쉬운 의존성 관리
- 내장 서버

#### Spring Framework vs Spring Boot - 기능적 측면

**Spring Framework**

POJO 기반의 Enterprise Application 개발을 쉽고 편하게 할 수 있도록 한다.

- Java Application을 개발하는데 필요한 하부구조(Infrastructure)를 포괄적으로 제공한다.
- Spring이 하부구조를 처리하기 때문에 개발자는 Application 개발에 집중할 수 있다.
- 동적인 웹 사이트를 개발하기 위한 여러 가지 서비스를 제공한다.

**Spring Boot**

스프링부트는 빠른시간에 애플리케이션이 제품이 될 수 있도록 하는 것을 목표로 한다. 

- Actuator: 애플리케이션을 고수준에서 모니터링하고 추적 할 수 있도록 해준다. 
- Embedded Server Integrations: 서버가 애플리케이션에 통합되기 때문에 우리는 서버에 설치되는 별도의 애플리케이션 서버를 가질 필요가 없다. 
- Default Error Handling

<br>

### Spring Boot Starter

Spring Boot의 스타터 프로젝트들은 특정 애플리케이션 개발을 빠르게 시작할 수 있도록 도와주는 프로젝트들이다.

> - spring-boot-starter-web-services: SOAP Web Services
> - spring-boot-starter-web: Web and RESTful applications 
> - spring-boot-starter-test: Unit testing and Integration Testing 
> - spring-boot-starter-jdbc: Traditional JDBC 
> - spring-boot-starter-hateoas: Add HATEOAS features to your services 
> - spring-boot-starter-security: Authentication and Authorization using Spring Security 
> - spring-boot-starter-data-jpa: Spring Data JPA with Hibernate 
> - spring-boot-starter-cache: Enabling Spring Framework’s caching support 
> - spring-boot-starter-data-rest: Expose Simple REST Services using Spring Data REST 

<br>

#### [참고]

> - https://shlee0882.tistory.com/200
> - https://ooz.co.kr/170
> - https://aahc.tistory.com/19
> - https://server-engineer.tistory.com/739

