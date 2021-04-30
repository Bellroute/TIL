# AOP(Aspect Oriented Programming)

AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다. 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다. 

Ex) 

- 핵심적인 관점 - 핵심 비즈니스 로직
- 부가적인 관점 - 데이터베이스 연결, 로깅, 파일 입출력

![img](https://t1.daumcdn.net/cfile/tistory/2473C33D58496A2A0F)

소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는데 이를 흩어진 관심사(Cross Cutting Concers)라 부름. AOP는 이런 <u>흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하는 것을 목적으로 사용</u>한다.

<br>

#### OOP vs AOP

- **OOP** : 비지니스 로직의 모듈화
  - 모듈화의 핵심 단위는 비지니스 로직
- **AOP** : 인프라 혹은 부가기능의 모듈화
  - 대표적 예 : 로깅, 트랜잭션, 보안 등
  - 각각의 모듈들의 주 목적 외에 필요한 부가적인 기능들

<br>

### AOP 주요 개념

- **Aspect** : 흩어진 관심사를 모듈화 한 것. Advice + Pointcut

- **Target** : Aspect를 적용하는 곳 (클래스, 메서드 .. )

- **Advice** : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체

- **JointPoint** : Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능

- **PointCut** : JointPoint의 상세한 스펙을 정의한 것. 'A란 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음. Jointpoint의 정규 표현식.
- **Weaving** : Aspect를 대상 객체에 연결시켜 관점지향 객체로 만드는 과정. Advice를 비즈니스 로직 코드에 삽입하는 것을 의미

<br>

### Spring AOP

- 프록시 패턴 기반의 AOP 구현체. 프록시 객체를 쓰는 이유는 접근 제어 및 부가 기능을 추가하기 위해서임

  `프록시 패턴(Proxy Pattern)` : 실제 기능을 수행하는 객체Real Object 대신 가상의 객체Proxy Object를 사용해 로직의 흐름을 제어하는 디자인 패턴. 어떤 기능을 추가하려 할 때 기존 코드를 변경하지 않고 기능을 추가할 수 있음.

  ```
              |---------------|      사용    |------|
              |Inteface       |<────────────|Client|
              |---------------|             |------|
              |+operation()   |
              |---------------|
                      △
                      │구현
          ┌────────────────────────┐
          │                        │
  |---------------|         |---------------|
  |Real Object    |   위임   |Proxy Object   |
  |---------------|<────────|---------------|
  |+operation()   |         |+operation()   |
  |---------------|         |---------------|
  ```

- 스프링 빈에만 AOP 적용 가능

- 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복 코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적

<br>

### Spring AOP 사용법

#### 의존성 추가

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

#### Aspect 클래스

```java
@Component
@Aspect
public class PerfAspect {

	@Around("execution(* com.saelobi..*.EventService.*(..))")
	public Object logPerf(ProceedingJoinPoint pjp) throws Throwable{
		long begin = System.currentTimeMillis();
		Object retVal = pjp.proceed(); // 메서드 호출 자체를 감쌈
		System.out.println(System.currentTimeMillis() - begin);
		return retVal;
	}
}

```

**@Aspect**

Aspect 로직을 담당할 클래스에 @Aspect를 붙여 이 클래스가 Aspect를 나타내는 클래스라는 것을 명시하고 @Component를 붙여 스프링 빈으로 등록한다.

**Advice**

어드바이스는 Aspect가 "무엇을", "언제"할지를 의미하는데, "무엇"은 해당 메소드에서 이루어지는 로직을 의미하고, "언제"는 총 5개의 타입이 존재한다.

- **@Before (이전)**
  - 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
- **@After (이후)**
  - 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료 되면 어드바이스 기능을 수행
- **@AfterReturning (정상적 반환 이후)**
  - 타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
- **@AfterThrowing (예외 발생 이후)**
  - 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
- **@Around (메소드 실행 전후)**
  - 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출전과 후에 어드바이스 기능을 수행

<br>

#### [참고]

> - https://jdm.kr/blog/235
> - https://dailyheumsi.tistory.com/202
> - https://jojoldu.tistory.com/71
> - https://engkimbs.tistory.com/746
> - https://logical-code.tistory.com/118