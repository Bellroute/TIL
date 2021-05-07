# 상태 패턴(State Pattern)

객체가 특정 상태에 따라 행위를 달리하는 상황에서 자신이 직접 상태를 체크하여 상태에 따라 행위를 호출하지 않고, 상태를 객체화하여 상태가 행동을 할 수 있도록 위임하는 패턴

![img](https://blog.kakaocdn.net/dn/byJzt7/btqwyCgz9cR/ZyzPnwEvvbVm4uwKupKArK/img.png)

-  특정 상태에 따라 행위를 달리하는 상황에서, 자신이 직접 상태를 체크하여 상태에 따라 행위를 호출하지 않고, 상태를 객체화 하여 상태가 행동을 할 수 있도록 위임하는 패턴
- 객체의 특정 상태를 클래스로 선언하고, 클래스에서는 해당 상태에서 할 수 있는 행위들을 메서드로 정의한다. 그리고 이러한 각 상태 클래스들을 인터페이스로 캡슐화 하여, 클라이언트에서 인터페이스를 호출하는 방식을 말한다.

<br>

### 상태 패턴 예시

#### 상황

- 형광등을 만들어보자.
- 꺼져있을 때 On을 누르면 켜지고, 켜져 있을 때 Off를 누르면 꺼진다.
- 켜져 있는 상태에서 On 버튼을 누르면 그대로 켜져 있고, 꺼져 있는 상태에서 Off 버튼을 눌러도 아무런 변화가 없다.

```java
public class Light {
  private static int ON = 0;
  private static int OFF = 1;
  private int state;
  
  public Light() {
    this.state = OFF;
  }
  
  public void onButtonPushed() {
    if (Sate == ON) {
      Sytem.out.println("반응 없음");
      return;
    }
    
    System.out.println("Light On!");
    state = ON;
  }
  
  public void offButtonPushed() {
    if (Sate == OFF) {
      Sytem.out.println("반응 없음");
      return;
    }
    
    System.out.println("Light Off!");
    state = OFF;
  }
}
```

#### 문제점

- '취침등' 상태를 추가한다고 하면?

- 형광등이 켜져 있는 경우에 On 버튼을 누르면 취침등 상태로 전환
- 취침등 상태에서 On 버튼을 누르면 켜져있는 일반 상태로 전환
- 꺼져 있는 경우에 On 버튼을 누르면 켜진 상태로 전환

```java
public class Light {
  private static int ON = 0;
  private static int OFF = 1;
  private static int SLEEPING = 2;
  private int state;
  
  public Light() {
    this.state = OFF;
  }
  
  public void onButtonPushed() {
    if (state == ON) {
      System.out.println("취침등 상태");
      state = SLEEPING;
      return;
    }
    
    if(state == SLEEPING){
      System.out.println("Light On!");
      state = ON;
      return;
    }
    
    System.out.println("Light On!");
    state = ON;
  }
  
  public void offButtonPushed() {
    if (Sate == OFF) {
      Sytem.out.println("반응 없음");
      return;
    }
    
    if(state == SLEEPING){
        System.out.println("Light Off!");
        state = ON;
    }
    
    System.out.println("Light Off!");
    state = ON;
  }
}
```

새로운 상태가 추가될 때마다 상태 변화를 초래하는 모든 메서드에 이를 반영하기 위해 코드 수정을 해야하는 문제 발생. 유지보수 힘들어짐.

#### 해결 방법

![img](https://media.vlpt.us/images/y_dragonrise/post/587e0780-77e3-4c96-bf61-44bf2fd2b400/image.png)

- 변하는 것은 잘 변하지 않는 것과 분리해라. 즉, 변하는 것을 캡슐화해라.
- 상태를 인터페이스로 분리시키고, 각 상태에 따른 구현 클래스를 만든다.

```java
interface State {
    public void onButtonPushed(Light light);
    public void offButtonPushed(Light light);
}
```

```java
public class OnState implements State {
  public void onButtonPushed(Light light) {
    System.out.println("취침등 상태");
    light.setState(new SleepingState(light));
  }
  
  public void offButtonPushed(Light light) {
    System.out.println("Light Off!");
    light.setState(new OffState(light));
  }
}


public class OffState implements State {
  public void onButtonPushed(Light light) {
    System.out.println("Light On!");
    light.setState(new OnState(light));
  }
  
  public void offButtonPushed(Light light) {
    System.out.println("반응 없음");
  }
}

public class SleepingState implements State {
  public void onButtonPushed(Light light) {
    System.out.println("Light On!");
    light.setState(new OnState(light));
  }
  
  public void offButtonPushed(Light light) {
    System.out.println("Light Off!");
    light.setState(new OffState(light));
  }
}
```

상태 패턴을 적용하여 유지보수가 용이해졌지만, 상태 변화가 생길 때마다 새로운 상태 객체를 생성하기 때문에 메모리 낭비로 인한 성능 저하를 야기한다. 이를 해결하기 위해서 싱글톤 패턴까지 적용하면 완벽!

<br>

#### 전략 패턴과 상태 패턴의 차이점

- 전략 패턴의 경우, 클라이언트 쪽에서 알고리즘을 변경하기 위하여 setter를 호출해 직접 수행할 알고리즘을 주입. 클라이언트가 구체적인 알고리즘의 수행까지는 몰라도 어느정도 무엇무엇이 있는지 정도는 알고 있어야 한다
- 상태 패턴의 경우, 각 상태 구현 클래스들이 자신들의 행위를 수행하면서 직접 Context(Light)객체의 상태를 변경해주기 때문에 클라이언트 입장에서는 직접 상태를 조작하거나 하지 않아도 된다. 즉, 클라이언트는 상태를 몰라도 된다

<br>

#### [참고]

> - https://www.crocus.co.kr/1541
> - https://velog.io/@y_dragonrise/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%83%81%ED%83%9C-%ED%8C%A8%ED%84%B4State-Pattern
> - https://coding-start.tistory.com/247



