# 어댑터 패턴(Adapter Pattern)

한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변환하는 패턴으로, 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 함게 작동하도록 해준다.

![img](https://t1.daumcdn.net/cfile/tistory/24231F4C575EACA210)



- Adapter 패턴은 Wrapper 패턴으로 불리기도 한다.

  - 일반 상품을 예쁜 포장지로 싸서 선물용 상품으로 만드는 것처럼, 무엇인가를 한번 포장해서 다른 용도로 사용할 수 있게 교환해주는 것이 wrapper이며 adapter이다.

- 외부 라이브러리를 사용할 때 활용될 수 있다.
  - 외부 라이브러리에서 제공하는 클래스가 충분한 테스트를 받아서 버그가 적어 재사용하고자 하는 경우
  - <u>기존의 클래스를 전혀 수정하지 않고</u>, 목적한 인터페이스에 맞추어 사용 가능하다.

<br>

### 어댑터 패턴 예시

#### 상황

- A사와 G사의 식권 발매 시스템이 있다고 하자.
- G사가 A사를 인수하고, 시스템을 통합하게 되었다.
- G사는 A사의 기존 시스템의 기능을 그대로 제공하면서, 몇 가지 기능을 좀 더 추가하려고 한다.
- A사의 기능
  - 식권 선택
  - 식권 출력
  - 구매(오프라인)
- G사의 기능
  - 식권 선택
  - 식권 출력
  - 오프라인 구매
  - 온라인 구매
  - 메뉴 들고오기

#### 문제점

[A사의 식권 시스템]

```java
public interface TicketA {
  public void chice(int token);
  public void print();
  public void buy();
}


public class TicketSystemA implements TicketA {
  @Override
  public void choice(int token) {
    System.out.println("선택된 식권 타입은... " + token + " 입니다.");
  }
  
  @Override
  public void print() {
    System.out.println("식권을 출력합니다..");
  }
  
  @Override
  public void buy() {
    System.out.println("식권을 구매합니다..");
  }
}
```

[G사의 식권 시스템]

```java
public interface TicketG {
  public void chice(int token);
  public void print();
  public void buyOnOffline();
  public void buyOnOnline();
  public String getMenu();
}


public class TicketSystemG implements TicketG {
  @Override
  public void choice(int token) {
    System.out.println("선택된 식권 타입은... " + token + " 입니다.");
  }
  
  @Override
  public void print() {
    System.out.println("식권을 출력합니다..");
  }
  
  @Override
  public void buyOnOffline() {
    System.out.println("오프라인으로 구매합니다..");
  }
  
  @Override
  public void buyOnOnline() {
    System.out.println("온라인으로 구매합니다..");
  }
  
  @Override
  public void getMenu() {
    System.out.println("메뉴정보를 DB에서 가져왔습니다.");
  }
}
```

기존 G사의 시스템 안에서 A사의 시스템이 정상적으로 돌아가야하기 때문에, 이걸을 해결하기 위해서는 A사의 인터페이스를 G사에 맞게 다시 정의해 주어야 한다. => 하지만 이 작업은 소스 전체의 중복을 야기한다. 시스템이 거대하다면 호환성을 위한 대대적인 공사는 많은 비용을 초래할 수도 있다.

#### 해결책

어댑터 클래스를 생성하여, 생성자로 A사의 TicketSystemA 클래스를 받고, G사의 인터페이스를 구현하여 그대로 사용할 부분에 A사의 메소드를 호출한다. 그리고 지원하지 않는 기능은 예외를 정의하여 처리한다.

[어댑터 클래스]

```java
public class TicketAdapter implements TicketG {
  private TicketA ticket;
  
  public TicketAdapter(TicketA ticket) {
    super();
    this.ticket = ticket;
  }
  
  @Override
  public void choice(int token) {
    this.ticket.choice(token);
  }
  
  @Override
  public void print() {
    this.ticket.print();
  }
  
  @Override
  public void buyOnOffline() {
    this.ticket.buy();
  }
  
  @Override
  public void buyOnOnline() {
    throw new UnsupportedOperationException("지원되지 않는 기능");
  }
  
  @Override
  public void getMenu() {
    throw new UnsupportedOperationException("지원되지 않는 기능");
  }
}
```

[실행]

```java
public class TicketMachine {
 
    public static void main(String[] args) {
        TicketG ticketG = new TicketAdapter(new TicketSystemA());
 
        ticketG.choice(1);
        ticketG.buyOnOffline();
        ticketG.print();
        try {
            System.out.println(ticketG.getMenu());
        }catch(UnsupportedOperationException e) {
            System.out.println("이 서비스는 G사의 다른 시스템에서 제공되는 기능입니다.");
        }
    }
}
```

<br>

### 유사한 디자인 패턴

#### Bridge 패턴

- Adapter 패턴은 인터페이스가 서로 다른 클래스들을 연결하는 패턴이다.
- Bridge 패턴은 기능의 계층과 구현의 계층을 연결시키는 패턴이다.

#### Decorator 패턴

- Adapter 패턴은 인터페이스의 차이를 조정하기 위한 패턴이다.
- Decorator 패턴은 인터페이스를 수정하지 않고 기능을 추가하는 패턴이다.

<br>

#### [참고]

> - https://gdtbgl93.tistory.com/141
> - https://jusungpark.tistory.com/22
> - https://niceman.tistory.com/141
> - https://opennote46.tistory.com/173
> - https://www.inflearn.com/course/Design-pattern-java/lecture/22159?tab=curriculum



