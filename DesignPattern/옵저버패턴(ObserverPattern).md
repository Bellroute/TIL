# 옵저버 패턴(Observer Pattern)

옵저버 패턴(observer pattern)은 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴

![Observer.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/854px-Observer.svg.png)

- 옵저버 패턴은 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 연락이 가고 자동으로 정보가 갱신되는 1:N의 관계를 정의
- 인터페이스를 이용하여 느슨한 결합성을 유지한다.
- 푸쉬 방식과 풀 방식 둘 다 가능
  - push 방식 : 데이터를 넘겨주는 방식이 주제 클래스에서 직접 넘겨주는 방식. 파라미터 사용
  - pull 방식 : 데이터를 얻어오는 방식이 옵저버 클래스에서 주제 클래스의 getter/setter 메소드를 통해 가져오는 방식. getter/setter 사용

#### 이용 사례

- 외부에서 발생한 이벤트(사용자 입력 등)에 대한 응답. 이벤트 기반 프로그래밍
- 객체의 속성 값 변화에 따른 응답.
- MVC 패턴에서 모델과 뷰 사이의 느슨한 연결을 위해 사용
- Java의 [Observer 인터페이스, Observable 클래스](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/Observable.html)

<br>

### 옵저버 패턴 예시

#### 상황

- 매일매일 새로운 뉴스를 제공하는 NewsMachine
- 정보를 제공받는 AnnualSubscriber, EventSubscriber
- 뉴스머신에서 나온 뉴스를 구독하고 싶어하는 Observer 인터페이스 => (AnnualSubscriber, EventSubscriber가 구현)
  - update() : 정보 업데이트
- Observer 객체들을 관리하는 Publisher 인터페이스=> (NewsMachine이 구현)
  - add() :  구독 등록
  - delete() : 구독 해지
  - notifyObserver() : 정보 공지

![img](https://t1.daumcdn.net/cfile/tistory/2214233B56A4D98611)

#### 코드

[인터페이스]

```java
// Observer 인터페이스
public interface Observer {
  public void update(String title, String news);
}

// Publisher 인터페이스
public interface Publisher {
  public void add(Observer observer);
  public void delete(Observer observer);
  public void notifyObserver();
}
```

[구현 클래스]

```java
public class NewsMachine implements Publisher { 
  private List<Observer> observers; 
  private String title; 
  private String news; 
  
  public NewsMachine() { 
    observers = new ArrayList<>(); 
  } 
  
  @Override public void add(Observer observer) {
    observers.add(observer); 
  } 
  
  @Override public void delete(Observer observer) { 
    int index = observers.indexOf(observer); 
    observers.remove(index); 
  } 
  
  @Override public void notifyObserver() { 
    for(Observer observer : observers) { 
      observer.update(title, news); 
    } 
  } 
  
  public void setNewsInfo(String title, String news) {
    this.title = title; 
    this.news = news; 
    notifyObserver(); 
  } 
}
```

```java
public class AnnualSubscriber implements Observer {
    private String newsString;

    @Override
    public void update(String title, String news) {
        this.newsString = title + " \n -------- \n " + news;
        display();
    }

    private void display() {
        System.out.println("\n\n오늘의 뉴스\n============================\n\n" + newsString);
    }
}
```

```java
public class EventSubscriber implements Observer {
    private String newsString;

    @Override
    public void update(String title, String news) {
        newsString = title + "\n------------------------------------\n" + news;
        display();
    }

    public void display() {
        System.out.println("\n\n=== 이벤트 유저 ===");
        System.out.println("\n\n" + newsString);
    }
}
```

[실행]

```java
public class MainClass {
    public static void main(String[] args) {
        NewsMachine newsMachine = new NewsMachine();
        Observer annualSubscriber = new AnnualSubscriber();
        Observer eventSubscriber = new EventSubscriber();
      
      	newsMachine.add(annualSubscriber);
      	newsMachine.add(eventSubscriber);
      
        newsMachine.setNewsInfo("오늘 한파", "전국 영하 18도 입니다.");
      
      	newsMachine.delete(annualSubscriber);
      
        newsMachine.setNewsInfo("벛꽃 축제합니다", "다같이 벚꽃보러~");
    }
}
```

<br>

#### [참고]

- https://flowarc.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4Observer-Pattern
- https://pjh3749.tistory.com/266
- https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4
- https://kscory.com/dev/design-pattern/observer
- https://coloredrabbit.tistory.com/86

