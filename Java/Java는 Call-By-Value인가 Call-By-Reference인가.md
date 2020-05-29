# Java는 Call-By-Value인가 Call-By-Reference인가?

함수의 호출 방식에는 Call By Value(값에 의한 호출) 방식과 Call By Reference(참조에 의한 호출) 방식이 있다. 

- `Call-By-Value (값에 의한 호출)` :  메서드 호출 시에 사용되는 인자의 메모리에 저장되어 있는 값(value)을 복사

- `Call-By-Reference (참조에 의한 호출)` : 메서드 호출 시에 사용되는 인자가, 값이 아닌 주소(Address)를 넘김

</br>

결론을 먼저 이야기하자면 **Java는 Call-By-Value 방식을 따른다.**

```java
public static void swap(int x, int y) {
    int temp = x;
    x = y;
    y = temp;
}

public static void main(String[] args) {
        int a = 10;
        int b = 20;
  
        System.out.println("before: a = " + a + ", b = " + b);
        swap(a, b);
        System.out.println("after: a = " + a + ", b = " + b);

    }

// 출력
// before: a = 10, b = 20
// after: a = 10, b = 20
```

swap()을 수행하기 전,후의 결과값이 동일함을 확인할 수 있다. 이유는 호출에 사용한 인자 a,b와 swap() 함수 내부의 x, y는 서로 다른 주소값을 가지고 있기 때문이다. swap()의 인자로 a와 b를 담게 되면 a, b의 값이 x, y에 각각 담기게 된다. 그 다음 내부 로직에 의해 x와 y의 값이 서로 바뀐 것이지 a와 b에는 아무런 영향이 가지 않은 것이다. 다시 말해서 Java는 Call-By-Value를 따른다는 것이다.

</br>

이렇게만 보면 Java의 함수 호출 방식은 논란의 여지 없이 Call-By-Value를 지원하는 것이라고 생각할 수 있다. 다음 코드를 확인해보자.

```java 
public class CallByReference {
    private int value;

    public void setValue(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

    public static void swap(CallByReference x, CallByReference y) {
        int temp = x.getValue();
        x.setValue(y.getValue());
        y.setValue(temp);
    }

    public static void main(String[] args) {
        CallByReference a = new CallByReference();
        a.setValue(10);

        CallByReference b = new CallByReference();
        b.setValue(20);

        System.out.println("before: a = " + a.value + ", b = " + b.value);
        swap(a, b);
        System.out.println("after: a = " + a.value + ", b = " + b.value);
    }

// 출력
// before: a = 10, b = 20
// after: a = 20, b = 10
```

이번에는 CallByReference라는 객체를 함수 인자로 넘겨주었다. 결과는 a, b의 값이 서로 변경되었다. 앞서 Java는 Call-By-Value라고 했는데 이 코드에서는 Call-By-Reference를 지원하는 것처럼 메모리 주소를 참조하였다. Java는 Call-By-Reference 또한 지원하는 것일까?

</br>

첫 번째와 두 번째 코드의 차이는 인자로 어떤 타입을 넘겨주었는가이다. Java의 자료형은 기본 자료형(Primitive Type)과 참조 자료형(Reference Type)이 있다. 기본 자료형은 int, long, float 등과 같이 값 자체를 가지고 있는 변수 타입이고 참조 자료형은 그 외에 new를 통해 객체를 생성하는 타입으로 클래스, 인터페이스, 배열 등이 해당한다. (String은 참조 자료형이지만 new를 쓰지 않아도되는 유일한 참조 자료형이다.)

\* 데이터를 비교할 때 기본 자료형은 값 자체를 비교하는데 반해 참조 자료형은 메모리 주소가 동일하지 않으면 다른 것으로 인식함

</br>

첫 번째 코드에서는 인자로 기본 자료형을 넘겨주었고 때문에 값 자체가 들어있었다. 반면 두 번째 코드에서는 인자로 참조 자료형을 넘겨주었다. 언뜻보면 참조에 의한 호출을 하는 것 같지만 사실은 참조 자료형은 객체의 주소를 값으로 가지고 있기 때문에 참조 값을 가지고 있는 것이다. 그렇기 때문에 이 역시 Call-By-Value로 봐야한다는 것이다. (Java가 Call-By-Reference라면 주소값을 가진 자료 참조형의 실제 주소 또한 알아낼 수 있어야한다.)

</br>

#### 결론

1. Java는 Call-By-Value이다.
2. 기본 자료형은 값 자체를 가지고, 참조 자료형은 주소를 값으로 가진다.
3. Call-By-Reference로 보이는 사례(참조 자료형을 인자로 받는 함수 호출) 역시 Call-By-Value이다.

</br>

### 참고

- [[Java] Call by value와 Call by reference](https://re-build.tistory.com/3)
- [자바의 메소드(함수) 호출 방식 - Call by Value vs Call by Reference](https://siyoon210.tistory.com/104)
- [자바의 아규먼트 전달 방식](https://brunch.co.kr/@kd4/2)
- [Java 인자 전달 방식: Call-by-{Value | Reference}?]([http://mussebio.blogspot.com/2012/05/java-call-by-valuereference.html](http://mussebio.blogspot.com/2012/05/java-call-by-valuereference.html))
- [자바 기초_3 참조 자료형 이해하기](https://niees.tistory.com/7)