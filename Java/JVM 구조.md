# JVM 구조

### JVM(Java Virtual Machine)?

> JVM은 운영체제 위에서 동작하는 프로세스로 자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 한다.

프로그램이 실행되기 위해서는 운영체제(OS)가 제어하고 있는 시스템의 리소스의 일부인 메모리(RAM: 주기억장치)를 제어할 수 있어야한다. 때문에 Java 이전의 C 같은 대부분의 언어로 만들어진 프로그램은 OS에 종속되어 실행되게 되어 있었다. 반면 Java 프로그램은 OS(플랫폼)과 독립적으로 동작이 가능한데 이를 가능하게 해준 것이 JVM이다.

JVM은 자바 가상 머신으로 **자바 바이트 코드를 실행할 수 있는 주체**이다. 쉽게 말해 JVM은 Java와 OS 사이에서 중계자 역할을 한다.  Java 프로그램을 OS에서 직접 실행시키는 것이 아니라, OS가 JVM을 실행시켜 메모리 사용권한을 할당하고 JVM이 Java 프로그램을 실행시키는 방식이다. 이러한 특징은 **속도가 느리다**는 단점이 있지만 Java는 **플랫폼으로부터 독립적**으로 동작이 가능하도록 한다. (Java는 플랫폼 독립적이지만 JVM은 플랫폼에 종속적임. 때문에 OS에 맞는 JVM을 설치해야한다.)



### JVM 메모리 구조

> Java에서 프로그램을 실행한다는 것은 컴파일 과정을 통하여 생성된 Class 파일을 JVM으로 로딩하고 ByteCode를 해석(interpret)하는 과정을 거쳐 메모리 등의 리소스를 할당하고 관리하며 정보를 처리하는 일련의 작업들을 포괄한다.

JVM은 크게 Class Loader, Execution Engine, Garbage Collector, Runtime Data Area로 구성되어 있다.

![img](https://t1.daumcdn.net/cfile/tistory/9973563D5ACE031521)

#### 1. Class Loader

**Java source**는 우리가 작성하는 자바 코드를 의미한다. .java 파일로 생성이 되는데, **Java Compiler**는 .java 파일을 Byte Code로 변환시켜 .class 파일을 생성한다. **Class Loader**는 <u>Class 파일을 OS로부터 할당받은 메모리(Runtime Data Area)에 적재하는 기능</u>을 한다. 



#### 2. Execution Engine

**Execution Engine**은 Class Loader에 의해 <u>메모리에 적재된 클래스들을 기계어로 변경해 명령어 단위로 실행</u>하는 역할을 한다. 명령어를 실행시키는 방법에는 두 가지 방식이 있다.

- `인터프리터(Interpreter) 방식` : 명령어를 순차적으로 읽으며 실행하는 방식
- `JIT(Just In Time) 방식` : 실행 시점에 자주 쓸만한 코드들을 기계어로 변환시켜놓고 저장해서 사용하는 방식

\* `JIT 방식`은 유사한 바이트코드를 매번 다시 컴파일하지 않고, 캐싱해둬서 컴파일에 필요한 총 시간을 단축한다.



#### 3. Garbage Collector

**Garbage Collector**는 <u>Heap 메모리 영역에 적재된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할</u>을 한다. 삭제된 객체의 메모리를 반환하여 Heap 메모리를 재사용함으로써 메모리를 효율적으로 사용하도록 한다. 단, Garbage Collector가 수행되는 정확한 시간은 알 수 없기 때문에 참조가 없어지자마자 해체되는 것을 보장하지 않는다. 또한 Garbage Collector가 수행되는 동안은 이를 수행하는 스레드가 아닌 다른 모든 스레드가 일시정지된다. (Heap 영역에 메모리가 계속해서 쌓여 속도가 저하된다던가 모든 스레드가 정지해 치명적인 장애로 이어질 수 있음)



#### 4. Runtime Data Area

**Runtime Data Area**는 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.

(이 부분은 따로 다루기로 하자)





### 참고

- [JVM 구조와 자바 런타임 메모리 구조 (자바 애플리케이션이 실행될 때 JVM에서 일어나는 일, 과정을 설명해줄 수 있나요?)](https://jeong-pro.tistory.com/148)
- [JVM 메모리구조](https://huelet.tistory.com/entry/JVM-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0)
- [[JVM Internal] JVM 메모리 구조](https://12bme.tistory.com/382)
- [JVM 구조 및 메모리](https://lazymankook.tistory.com/79)
- [JVM 구조](https://velog.io/@litien/JVM-%EA%B5%AC%EC%A1%B0)

