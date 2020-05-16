# String vs StringBuffer vs StringBuilder

Java에서 문자열을 다루기 위해 주로 String, StringBuffer, StringBuilder라는 클래스를 사용하게 된다. 비슷하지만 다른 이 클래스들의 특징과 차이점을 찾아보고 각 클래스들이 어느 상황에 적절하게 사용될 수 있는지 알아보고자 한다.



### String

String 문자열을 다룰 때 가장 일반적으로 사용하는 클래스로 **immutable(불변)**하다. String 객체를 메모리에 한 번 할당하면 그 공간이 변하지 않다는 뜻이다. String으로 생성된 데이터는 **Heap 영역**에 할당된다.

```java
String str = "ABC";
str += "DEF"
```

위의 예시는 String에 문자열을 추가하는 일반적인 경우로 str는 "ABCDEF"가 된다. 언뜻보면 원래의 문자열에 "DEF"를 추가한 것처럼 보이지만 String은 내부 데이터를 수정할 수 없기 때문에 "ABCDEF"라는 데이터를 가진 새로운 String 객체를 생성 str가 새로 생성된 객체를 참조하는 것이다. 기존에 있던 "ABC"의 공간은 참조하는 변수가 없기 때문에 가비지 컬렉션에 의해 회수된다.

![출처-코딩팩토리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FY5uyc%2FbtqEa0VABIn%2FqyBqm9kXuxqWju1HXX83C0%2Fimg.png)

String의 이런 특징 때문에 문자열 연산이 많은 경우에는 String은 적합하지 않다. 연산이 많아진다는 건 그만큼 가비지 컬렉션에 의해 회수되어야할 공간이 많아진다는 것이고 회수될 때까지 계속해서 쌓이기 때문에 메모리 부족으로 속도가 느려질 수 있기 때문이다.

하지만, Imuutable한 객체는 간단하게 사용가능하고, 동기화에 대해 신경쓰지 않아도 되기 때문에(Thread-Safe), 내부 데이터를 자유롭게 공유할 수 있다.



### StringBuffer, String Builder

String에 반해 StringBuffer와 StringBuilder는 **mutable(가변)**한 클래스로 할당 받은 메모리의 크기를 유연하게 바꿀 수 있고, 제공하는 메서드 역시 동일하다. 그럼에도 다른 객체를 만들어 놓은 이유는 **동기화 여부**에 있다.

StringBuffer는 각 메서드 별로 Synchronized Keyword가 존재하여, 멀티스레드 환경에서도 동기화를 지원한다(thread-safe). 하지만 StringBuilder는 동기화를 보장하지 않는다.

따라서 멀티스레드 환경이라면 동기화 보장을 위해 StringBuffer를 사용하고, 단일스레드 환경이라면 StringBuilder를 사용하는 것이 좋다. 단일스레드 환경에서 StringBuffer를 사용한다고 문제가 되는 것은 아니지만, 동기화 관련 처리로 인해 StringBuilder에 비해 성능이 떨어질 수 있다.



\* JDK 1.5 버전 이후 컴파일 단계에서 String을 사용한다하더라도 StringBuilder로 컴파일되도록 변경되어 성능상 차이가 없다고 한다. (하지만, 실제로는 그렇지 않은 듯 하다. https://devhong.tistory.com/27)



### 참고

> - [[Java] String, StringBuffer, StringBuilder의 차이점과 사용이유](https://coding-factory.tistory.com/546)
> - [String, StringBuffer, StringBuilder 차이점과 장단점](https://ooz.co.kr/298)
> - [Java String , StringBuffer , StringBuilder 란 무엇인가?](https://zzdd1558.tistory.com/144)
> - [String StringBuffer StringBuilder 차이]( https://devhong.tistory.com/27)