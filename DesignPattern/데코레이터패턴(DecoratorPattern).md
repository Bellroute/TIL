# 데코레이터 패턴(Decorator Pattern)

객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴

![img](https://gmlwjd9405.github.io/images/design-pattern-decorator/decorator-pattern.png)

- 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계하는 방식
- 기본 기능에 추가할 수 있는 많은 종류의 부가 기능에서 파생되는 다양한 조합을 동적으로 구현할 수 있는 패턴
- 구조(Structural) 패턴의 하나
  - `구조 패턴?` : 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴

<br>

### 데코레이터 패턴 예시

#### 상황

- 네비게이션 SW에서 도로를 표시하는 기능을 설계
- 기본 기능 - 도로를 간단한 선으로 표시
- 추가 기능 - 도로의 차선을 표시하는 기능

```java
// 기본 도로 표시 클래스
public class RoadDisplay {
    public void draw() { System.out.println("기본 도로 표시"); }
}


// 기본 도로 표시 + 차선 표시 클래스
public class RoadDisplayWithLane extends RoadDisplay {
  public void draw() {
      super.draw(); // 상위 클래스, 즉 RoadDisplay 클래스의 draw 메서드를 호출해서 기본 도로 표시
      drawLane(); // 추가적으로 차선 표시
  }
  private void drawLane() { System.out.println("차선 표시"); }
}


public class Client {
  public static void main(String[] args) {
      RoadDisplay road = new RoadDisplay();
      road.draw(); // 기본 도로만 표시

      RoadDisplay roadWithLane = new RoadDisplayWithLane();
      roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
  }
}
```

#### 문제점

- 또 다른 도로 표시 기능을 추가로 구현하게 된다면? 그리고 추가 기능들을 조합해야한다면?

![img](https://gmlwjd9405.github.io/images/design-pattern-decorator/decorator-problem3.png)

- 3개의 추가 기능이 있다면 조합별로 총 7개의 하위 클래스를 구현해야 한다.

- 즉, 다양한 기능의 조합을 고려해야 하는 경우 상속을 통한 기능의 확장은 각 기능별로 클래스를 추가해야 한다 는 단점이 있음

#### 해결책

- 각 추가 기능별로 개별적인 클래스를 설계하고 기능을 조합할 때 각 클래스의 객체 조합을 이용

  ![img](https://gmlwjd9405.github.io/images/design-pattern-decorator/decorator-solution.png)

```java
public abstract class Display { public abstract void draw(); }


/* 기본 도로 표시 클래스 */
public class RoadDisplay extends Display {
  @Override
  public void draw() { System.out.println("기본 도로 표시"); }
}


/* 다양한 추가 기능에 대한 공통 클래스 */
public abstract class DisplayDecorator extends Display {
  private Display decoratedDisplay;
  // '합성(composition) 관계'를 통해 RoadDisplay 객체에 대한 참조
  public DisplayDecorator(Display decoratedDisplay) {
      this.decoratedDisplay = decoratedDisplay;
  }
  @Override
  public void draw() { decoratedDisplay.draw(); }
}


/* 차선 표시를 추가하는 클래스 */
public class LaneDecorator extends DisplayDecorator {
  // 기존 표시 클래스의 설정
  public LaneDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
  @Override
  public void draw() {
      super.draw(); // 설정된 기존 표시 기능을 수행
      drawLane(); // 추가적으로 차선을 표시
  }
  // 차선 표시 기능만 직접 제공
  private void drawLane() { System.out.println("\t차선 표시"); }
}


/* 교통량 표시를 추가하는 클래스 */
public class TrafficDecorator extends DisplayDecorator {
  // 기존 표시 클래스의 설정
  public TrafficDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
  @Override
  public void draw() {
      super.draw(); // 설정된 기존 표시 기능을 수행
      drawTraffic(); // 추가적으로 교통량을 표시
  }
  // 교통량 표시 기능만 직접 제공
  private void drawTraffic() { System.out.println("\t교통량 표시"); }
}


public class Client {
  public static void main(String[] args) {
      Display road = new RoadDisplay();
      road.draw(); // 기본 도로 표시
    
      Display roadWithLane = new LaneDecorator(new RoadDisplay());
      roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
    
      Display roadWithTraffic = new TrafficDecorator(new RoadDisplay());
      roadWithTraffic.draw(); // 기본 도로 표시 + 교통량 표시
    
			Display roadWithLaneAndTraffic = new TrafficDecorator(
        															 new LaneDecorator(
      																 new RoadDisplay()));
			roadWithLaneAndTraffic.draw(); // 기본 도로 표시 + 차선 표시 + 교통량 표시
  }
}
```

- 어떤 기능을 추가하느냐에 관계 없이 Client 클래스는 동일한 Display 클래스만을 통해 일관성 있는 방식으로 도로 정보를 표시함

- 추가 기능 조합별로 별도의 클래스를 구현하는 대신 각 추가 기능에 해당하는 클래스의 객체를 조합해 추가 기능의 조합을 구현할 수 있다.

- 데코레이터 패턴이 적용된 예로는 자바 입출력의 필터 스트림이 있음.

  ```java
  // Java File IO
  BufferedReader br = null;
  try {
    br = new BufferedReader(new FileReader(new File("test.txt")));
  } catch (Exception e) {
    e.printStackTrace();
  }
  ```

<br>

### 데코레이터 패턴의 단점

- 데코레이터 패턴을 사용하면 자잘한 객체들이 매우 많이 추가될 수 있고, 데코레이터를 너무 많이 사용하면 코드가 필요 이상으로 복잡해질 수도 있다.

- 구성요소(Component)를 초기화하는 데 필요한 코드가 훨씬 복잡해진다. 컴포턴트 instance만 만든다고 해서 일이 끝나는게 아니라 많은 데코레이터로 Wrapping해야하는 경우가 있다. 그래서 데코레이터 패턴은 팩토리 패턴이나 빌더 패턴과 함께 사용된다.

- 데코레이터 패턴은 구상 구성요소(Concrete Component)의 형식을 알아내서 그 결과를 바탕으로 어떤 작업을 처리하는 코드(특정 형식에 의존하는 클라이언트 코드)에는 적용할 수 없다. 기존의 구성요소가 RoadDisplay였을 때 기존의 구성요소를 데코레이터로 감싸게 되면, 그 구성요소가 RoadDisplay인지 아닌지를 알 수가 없게 된다.

<br>

#### [참고]

- https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html
- https://www.inflearn.com/course/Design-pattern-java/lecture/22161?tab=note

