# JVM - Runtime Data Area

Runtime Data Area는 JVM이 프로그램을 수행하기 위해 운영체제로부터 할당 받는 메모리 영역이다. Runtime Data Areas는 각각의 목적에 따라 5 개의 영역으로 나뉘게 된다.

- PC Registers
- JVM Stacks
- Native Method Stacks
- Method Area
- Heap

PC Register와 JVM Stack, Native Method Stack은 Thread 별로 생성되고, Method Area와 Heap은 모든 Thread에 공유된다.

</br>

### PC Register

> 현재 수행중인 JVM Instruction의 주소를 가지는 역할
>
> (PC Register는 Thread마다 독립적으로 존재하며, 각 스레드가 실행하고 있는 메소드에서 몇 번째 줄을 실행해야 하는지 나타냄)

**PC Register는 Register-Base가 아닌 Stack-Base로 작동한다.**

프로그램의 실행은 CPU에서 명령(Instruction)을 수행하는 과정으로 이루어진다. CPU는 이러한 명령을 수행하는 동안 필요한 정보를 레지스터라고 하는 CPU 내의 기억장치(=register)를 사용하는데 이와 같은 방법을 Register-Base라 한다.

![registerAdd](https://markfaction.files.wordpress.com/2012/07/registeradd_thumb.png?w=456&h=224)

```
1. ADD R1, R2, R3 ;        # Add contents of R1 and R2, store result in R3
```

그림을 보면 알 수 있듯이 Register 기반 모델에서는 한 줄의 명령으로 연산을 수행한다는 점에서 간단하지만, 피연산자들의 주소를 모두 명시적으로 참조해야한다. (Stack 모델에 비해 명령이 짧고, 오버헤드 적고, 빠름)

반면, Java의 PC Register는 Stack-Base로 작동한다. <u>JVM은 CPU에 직접 Instruction을 수행하지 않고 Stack에서 Operand를 뽑아 내어 이를 별도의 메모리 공간에 저장하는 방식을 취한다.</u>

![stackAdd](https://markfaction.files.wordpress.com/2012/07/stackadd_thumb.png?w=356&h=133)

```
1. POP 20
2. POP 7
3. ADD 20, 7, result
4. PUSH result
```

Stack 기반 모델에서는 데이터를 꺼내서 처리하고 결과를 LIFO방식으로 밀어 넣는 방식으로 수행되기 때문에, POP이나 PUSH와 같은 연산 수행으로 오버헤드가 발생하지만, 모든 피연산자들에 주소를 부여할 필요없이 Stack pointer만 이용하면 된다. (피연산자들의 주소를 전부 명시하지 않아도 됨. 추상화 수월)

</br>

**왜 Stack-Base를 선택했나?**

자바는 다기종의 디바이스에서 균일하게 동작할 수 있음을 보장하고자 했다. 디바이스마다 레지스터 수는 달라서 가정할 수 없었음. 그래서 레지스터 기반으로 하게되는 순간 하드웨어의 구현에 종속된다. 스택을 쓰게되면 계산과정은 복잡할 수 있어도 하드웨어의 스펙에 최소한의 관여만 할 수 있게 된다.

</br>

### JVM Stacks

> Thread 별로 하나씩 존재하며, 메소드가 호출될 때 수행 정보(메소드 호출 주소, 매개 변수, 지역 변수, 연산 스택)가 프레임(Frame)이라는 단위로 추가(push)하고 메소드가 종료되면 해당 프레임을 제거(pop)하는 동작을 수행
>
> Thread가 쓸 수 있는 스택의 사이즈를 넘기게 되면 `StackOverflowError`가 발생한다.

**stack frame**

- 프레임은 메소드가 호출될 때마다 만들어지며, 메소드 상태 정보를 저장한다.

- 다음의 3가지로 구성되어 있다.

  ![img](https://blog.kakaocdn.net/dn/238XL/btqzZc6Q9rT/5t4XaiOOs6ZaWRZYgQDo20/img.png)

  - **Local Variables** 
    - 메소드의 지역 변수(파라미터 포함)들을 저장
  - **Operand Stack**
    - 메소드 내 계산을 위한 작업 공간.
    - CPU Resgister와 유사
    - 대부분의 Bytecode Instruction은 값을 Local Variable과 Operand Stack 사이에서 양방향으로 옮기는 작업을 포함 ([참고](https://mia-dahae.tistory.com/108))
  - **Frame Data** 
    - Constant pool 정보 (== constant pool의 포인터 정보)
    - Normal Method Return (method가 정상 종료 했을 때의 정보들)
    - Exception Dispatch (비정상 종료했을 때 발생하는 Exception 관련 정보들)



### Method Area

> Class Loader가 적재한 클래스(또는 인터페이스)에 대한 메타데이터 정보가 저장되는 영역
>
> JVM이 시작할 때 생성되고 모든 스레드가 공유하는 영역으로, 
> 런타임 상수풀(runtime constant pool), 필드(field) 데이터, 메소드(Method) 데이터, 메소드 코드, 생성자(constructor) 코드 등 클래스와 인터페이스와 관련된 데이터들을 분류하고 저장한다.
>
> 프로그램의 시작부터 종료가 될 때까지 메모리에 남아있음.

- Type Information: 클래스와 관련한 모든 정보
- Constant Pool : 상수로 선언한 데이터들이 저장되는 곳
- Field Information : 클래스의 멤버 변수
- Method Information : Class 멤버메서드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보
- Static Variable: static으로 선언되는 모든 클래스 변수

</br>

### Heap

>  Heap 영역은 동적으로 생성 된 객체 또는 배열, Array 등을 저장하는 영역. 
>
> 메소드 영역에서 참조한 값을 바탕으로 새로운 객체를 생성 할 시에 이곳에 저장됨. 여기서 사용이 끝난 객체들을 Gabage Collector가 모아서 처리하게 됩니다.

</br>

### Native Method Stacks

>  자바 외 언어(C, C++ 등)을 수행하기 위한 Stack 영역. 
>
> 스레드별로 하나씩 존재하며, 프로그램 실행 도중 호출 된 메서드가 Native 방식을 사용하는 메소드 일 경우, 이 영역에 저장되어 처리됨.

</Br>

#### [참고]

> - https://markfaction.wordpress.com/2012/07/15/stack-based-vs-register-based-virtual-machine-architecture-and-the-dalvik-vm/
> - https://blog.embian.com/61
> - https://m.blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=220950553078&proxyReferer=https:%2F%2Fwww.google.com%2F
> - https://jithub.tistory.com/40
> - https://www.youtube.com/watch?v=UzaGOXKVhwU
> - https://www.slipp.net/wiki/pages/viewpage.action?pageId=8880255
> - https://technote.kr/310
> - https://www.holaxprogramming.com/2013/07/16/java-jvm-runtime-data-area/
> - https://mia-dahae.tistory.com/108
> - https://johngrib.github.io/wiki/jvm-stack/
> - https://m.blog.naver.com/PostView.nhn?blogId=2000yujin&logNo=130156226754&proxyReferer=https:%2F%2Fwww.google.com%2F
> - https://re-build.tistory.com/2