# JVM 구조

### JVM(Java Virtual Machine)?

> JVM은 운영체제 위에서 동작하는 프로세스로 자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 한다.

프로그램이 실행되기 위해서는 운영체제(OS)가 제어하고 있는 시스템의 리소스의 일부인 메모리(RAM: 주기억장치)를 제어할 수 있어야한다. 때문에 Java 이전의 C 같은 대부분의 언어로 만들어진 프로그램은 OS에 종속되어 실행되게 되어 있었다. 반면 Java 프로그램은 OS(플랫폼)과 독립적으로 동작이 가능한데 이를 가능하게 해준 것이 JVM이다.

JVM은 자바 가상 머신으로 **자바 바이트 코드를 실행할 수 있는 주체**이다. 쉽게 말해 JVM은 Java와 OS 사이에서 중계자 역할을 한다.  Java 프로그램을 OS에서 직접 실행시키는 것이 아니라, OS가 JVM을 실행시켜 메모리 사용권한을 할당하고 JVM이 Java 프로그램을 실행시키는 방식이다. 이러한 특징은 **속도가 느리다**는 단점이 있지만 Java는 **플랫폼으로부터 독립적**으로 동작이 가능하도록 한다. (Java는 플랫폼 독립적이지만 JVM은 플랫폼에 종속적임. 때문에 OS에 맞는 JVM을 설치해야한다.)</br>

#### JVM < JRE < JDK

![Image for post](https://miro.medium.com/max/1932/1*KMgwPmc_uogTbqkbJg1ntg.jpeg)

**JRE(Java Runtime Envirment)**

- 자바 실행 환경
- JRE는 JVM 이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다. 
- JRE는 JVM의 실행환경을 구현했다고 할 수 있다.
- 즉, 자바 프로그램을 실행시키기 위해서는 JRE를 설치해야 한다.

**JDK(Java Develpment Kit)**

- 자바 개발 도구
- JDK는 JRE + 개발을 위해 필요한 도구(javac, java등)들을 포함한다.
- JDK를 설치하면 JRE도 같이 설치된다.
- JDK 11 버전부터는 JRE를 포함하지 않는다고 한다.
  - 모듈화을 통해 애플리케이션에 필요한 구성 요소만 포함하는 런타임 구성을 사용자 지정할 수 있도록 하여, 메모리 공간을 더 효율적으로 사용할 수 있도록 하기 위함 (마이크로서비스 아키텍처에 유용)
  - [jlink](https://docs.oracle.com/en/java/javase/11/tools/jlink.html)를 사용하여 배포용 사용자 지정 런타임에 정적으로 연결

</br>

### JVM 메모리 구조

> Java에서 프로그램을 실행한다는 것은 컴파일 과정을 통하여 생성된 Class 파일을 JVM으로 로딩하고 ByteCode를 해석(interpret)하는 과정을 거쳐 메모리 등의 리소스를 할당하고 관리하며 정보를 처리하는 일련의 작업들을 포괄한다.

JVM은 크게 Class Loader, Execution Engine, Garbage Collector, Runtime Data Area로 구성되어 있다.

#### 1. Class Loader

**Java source**는 우리가 작성하는 자바 코드를 의미한다. .java 파일로 생성이 되는데, **Java Compiler**는 .java 파일을 Byte Code로 변환시켜 .class 파일을 생성한다. **Class Loader**는 <u>Class 파일을 OS로부터 할당받은 메모리(Runtime Data Area)에 적재하는 기능</u>을 한다. 

<br>

#### 2. Execution Engine

**Execution Engine**은 Class Loader에 의해 <u>메모리에 적재된 클래스들을 기계어로 변경해 명령어 단위로 실행</u>하는 역할을 한다. 명령어를 실행시키는 방법에는 두 가지 방식이 있다.

- `인터프리터(Interpreter) 방식` : 명령어를 순차적으로 읽으며 실행하는 방식
- `JIT(Just In Time) 방식` : 실행 시점에 자주 쓸만한 코드들을 기계어로 변환시켜놓고 저장해서 사용하는 방식

\* `JIT 방식`은 유사한 바이트코드를 매번 다시 컴파일하지 않고, 캐싱해둬서 컴파일에 필요한 총 시간을 단축한다.

</br>

#### 3. Garbage Collector

**Garbage Collector**는 <u>Heap 메모리 영역에 적재된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할</u>을 한다. 삭제된 객체의 메모리를 반환하여 Heap 메모리를 재사용함으로써 메모리를 효율적으로 사용하도록 한다. 단, Garbage Collector가 수행되는 정확한 시간은 알 수 없기 때문에 참조가 없어지자마자 해체되는 것을 보장하지 않는다. 또한 Garbage Collector가 수행되는 동안은 이를 수행하는 스레드가 아닌 다른 모든 스레드가 일시정지된다. (Heap 영역에 메모리가 계속해서 쌓여 속도가 저하된다던가 모든 스레드가 정지해 치명적인 장애로 이어질 수 있음)

</br>

#### 4. Runtime Data Area

**Runtime Data Area**는 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.

- **Method Areas** : 클래스, 변수, Method, static 변수, 상수 정보 등이 저장되는 영역이다.(모든 Thread가 공유한다.)

- **Heap Area** : new 명령어로 생성된 인스턴스와 객체가 저장되는 구역이다.(Garbage Collection 이슈는 이 영역에서 일어나며, 모든 Thread가 공유한다.)

- **Stack Area** : Java Method 내에서 사용되는 값들(매개변수, 지역변수, 리턴값 등)이 저장되는 구역으로 메소드가 호출될 때 LIFO로 하나씩 생성되고, 메소드 실행이 완료되면 LIFO로 하나씩 지워진다.(각 Thread별로 하나씩 생성된다.)

- **PC Register** : CPU의 Register와 역할이 비슷하다. 현재 수행 중인 JVM 명령의 주소값이 저장된다.(각 Thread별로 하나씩 생성된다.)

- **Native Method Stack** : 다른 언어(C/C++ 등)의 메소드 호출을 위해 할당되는 구역으로 언어에 맞게 Stack이 형성되는 구역이다.

</br>

### 자바 프로그램 실행 과정

![img](https://t1.daumcdn.net/cfile/tistory/9973563D5ACE031521)

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다. JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스 코드(.java)를 읽어들여 자바 바이트 코드(.class)로 변환시킨다.
3. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.
   - JVM 내의 Runtime Data Area에 바이트 코드를 배치시키고 이것은 실행 엔진에 의해 실행된다.
4. 로딩된 class 파일들은 Execution Engine을 통해 해석된다. 2가지 방식이 존재한다.
   - `인터프리터 방식` : 최초의 방식. 자바 바이트 코드를 명령어 단위로 읽어서 실행. 한 줄씩 실행하기 때문에 느리다.
   - `JIT(Just-In-Time) Compiler` : 인터프리터 방식의 단점을 보완하기 위해 등장
     - 인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경하고 이후에는 더 이상 인터프리팅하지 않고 네이티브 코드로 직접 실행하는 방식이다.
     - 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 빠르게 수행된다.
     - 한 번만 실행되는 코드라면 JIT 컴파일러가 컴파일하는 게 인터프리팅하는 것보다 오래 걸리므로 인터프리팅하는 것이 유리하다.
     - 이처럼 해당 메소드가 얼마나 자주 수행되는지 체크하고 일정 정도를 넘을 때만 컴파일을 수행하는 게 좋다.
5. 해석된 바이트 코드는 Runtime Data Area(JVM이 프로그램을 수행하기 위해 OS로부터 할당받은 메모리 공간)에 배치되어 실질적인 수행이 이루어지게 된다. 이러한 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC 같은 관리 작업을 수행한다.

</br>

### 참고

- [JVM 구조와 자바 런타임 메모리 구조 (자바 애플리케이션이 실행될 때 JVM에서 일어나는 일, 과정을 설명해줄 수 있나요?)](https://jeong-pro.tistory.com/148)
- [JVM 메모리구조](https://huelet.tistory.com/entry/JVM-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0)
- [[JVM Internal] JVM 메모리 구조](https://12bme.tistory.com/382)
- [JVM 구조 및 메모리](https://lazymankook.tistory.com/79)
- [JVM 구조](https://velog.io/@litien/JVM-%EA%B5%AC%EC%A1%B0)
- [JAVA 11로 전환해야 하는 이유](https://docs.microsoft.com/ko-kr/azure/developer/java/fundamentals/reasons-to-move-to-java-11)
- [JDK와 JRE 차이점](https://goodgid.github.io/Java-JDK-JRE/)

