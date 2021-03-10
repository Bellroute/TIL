# JVM - Execution Engine

Class Load 작업이 끝난 Class 파일은 Runtime Data Area의 Method Area에 배치된다. JVM은 Method Area의 Byte Code를 Execution Engine에 제공하여 Class에 정의된 내용대로 실행하게 된다.

이때, <u>로드된 Class의 ByteCode를 실행하는 Runtime Module이 바로 Execution Engine이다.</u>

- ByteCode는 JVM이 실행하는 명령어(Insturction) 집합이다. 자바 바이트코드는 `.class`가 담고 있다. 

- Execution Engine이 수행하는 가장 최소 단위의 명령어를 Instruction이라고 하고, 1바이트의 Opcode(operation code의 약자)와 Operand로 구성되어 있다.

</br>

#### Execution Engine의 방식

바이트 코드를 실행하기 위해 기계어로 변환하는 방법에 따라 2가지 방식으로 나뉜다.

**1. Interpreter 방식**

- <u>바이트코드를 한 줄씩 해석, 실행하는 방식이다.</u>
- 동일 메서드가 여러번 호출되더라도 매번 해석하고 실행시킴
- 초기 방식으로, 한 줄에 대한 해석을 빠른 대신 전체 결과의 실행은 느리다.

**2. JIT(Just In Time) 컴파일 방식**

- Interperter 방식의 단점을 보완해 등장하는 것이 JIT(Just In Time) 컴파일 방식이다. 
- <u>적절한 시점에 ByteCode를 Compile하여 각 OS에 맞는 Native Code로 변경하고, Native Code로 직접 실행하는 방식.</u>
- 또한, JIT 컴파일러는 같은 코드를 매번 해석하지 않고, 실행할 때 컴파일을 하면서 해당 코드를 캐싱해버린다. 이후에는 바뀐 부분만 컴파일하고, 나머지는 캐싱된 코드를 사용한다. 캐싱된 코드는 코드 캐시라는 곳에 저장된다.
- 바이트코드를 JIT 컴파일러를 이용해 프로그램을 실제 실행하는 시점(바이트코드를 실행하는 시점)에 각 OS에 맞는 Native Code로 변환하여 실행 속도를 개선하였다. 

- 하지만, 바이트코드를 Native Code로 변환하는 데에도 비용이 소요됨. 때문에 반복 수행이 발생하지 않으면 Interpreter보다 느림.
- 때문에 JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다 일정 기준이 넘어가면 JIT 컴파일 방식으로 명령어를 실행한다. 

**JIT 컴파일에서 말하는 적절한 시점?**

- 적절한 시점 == 자주 호출되는 코드임을 확인
- Execution Engine은 JVM 옵션을 통해 튜닝이 가능하다.
- 컴파일 임계치에 임의의 값을 지정하게되면 코드 호출 빈도가 해당 임계치에 도달하면 바이트 코드를 컴파일하는 방식으로 많이 사용된다는 기준을 설정할 수 있다.
- 컴파일 임계치 = method entry counter(호출 빈도) + back-edge loop counter(루프 횟수)

**3. AOT 컴파일 방식**

- 프로그램 최초 실행시가 아닌, 그 이전에(주로 설치시에) 한번에 전체를 변환해 두고 저장한 뒤, 프로그램 실행시 마다 변환된 코드를 읽어들이게 된다
- 안드로이드에서 주로 사용되는데, JIT 컴파일러가 돌아가는데 실행시마다 하드웨어 전체적으로 상당한 부하가 생겨 배터리 성능 문제를 해결하기 위해 등장함.
- 속도 측면에서 성능 개선을 기대할 수 있지만, 메모리 공간을 많이 차지하게 된다.

</br>

#### 기타

- 구글링을 통해 확인 가능한 JVM 구조도를 보면 Garbage Collector가 Execution Engine에 포함되는 그림도 있고, 그렇지 않은 경우도 있었다.

</br>

#### [참고]

> - https://www.javacodegeeks.com/2018/04/jvm-architecture-execution-engine-in-jvm.html
> - https://www.slipp.net/wiki/pages/viewpage.action?pageId=8880270
> - https://velog.io/@prayme/.java%EB%A5%BC-JVM%EC%9C%BC%EB%A1%9C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B3%BC%EC%A0%95-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0
> - https://junshock5.tistory.com/111
> - https://namu.wiki/w/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%20%EB%9F%B0%ED%83%80%EC%9E%84