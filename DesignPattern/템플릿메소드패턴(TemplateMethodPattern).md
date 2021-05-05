# 템플릿 메소드 패턴(Template Method Pattern)

알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의하는 패턴.

![img](https://gmlwjd9405.github.io/images/design-pattern-template-method/template-method-pattern.png)

- 알고리즘이 단계별로 나누어 지거나, 같은 역할을 하는 메소드이지만 여러곳에서 다른 형태로 사용이 필요한 경우 유용한 패턴 - GoF

- 상속을 통해 슈퍼 클래스의 기능을 확장할 때 사용하는 가장 대표적인 방법. 변하지 않는 기능은 슈퍼 클래스에 만들어두고 자주 변경되며 확장할 기능은 서브클래스에서 만들도록 함 - 토비의 Spring 3.1
- 행위(Behavioral) 패턴 중 하나

전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화할 때 유용하다.

<br>

### 템플릿 메소드 패턴 예시

#### 상황

- 집을 건축하는 소스 코드
- 기반을 세우고, 창문을 다는 행위는 동일
- 벽과 기둥을 유리로 지을 것인지, 나무로 지을 것인지에 따라 형태가 달라짐

#### 소스 코드

상위 클래스에 해당하는 `HouseTemplate` 클래스에 변하지 않는 기능을 구현하고, 변경될 기능은 추상 메소드로 선언한다.

```java
public abstract class HouseTemplate {
  
  // final 선언으로 Override 방지
  public final void buildHouse() {
    buildFoundation();
    buildPillars();
    buildWalls();
    buildWindows();
    System.out.println("House is built.");
  }
  
  // 기본으로 구현
  private void buildWindows() {
    System.out.println("Building Glass Windows");
  }
  
  // 기본으로 구현
  private void buildFoundation() {
    System.out.println("Building foundation with cement, iron rods and sand");
  }
  
  // 서브 클래스에서 직접 구현할 메소드
  protected abstract void buildWalls();
  protected abstract void buildPillars();
}
```

하위 클래스인 `GlassHouse`와 `WoodenHouse`에서 추상 메소드를 구현한다.

```java
public class GlassHouse extends HouseTemplate {
  
  @Override
  public void buildWalls() {
    System.out.println("Building Glass Walls");
  }
  
  @Override
  public void buildPillars() {
    System.out.println("Building Pillars with glass coating");
  }
}


public class WoodenHouse extends HouseTemplate {
  
  @Override
  public void buildWalls() {
    System.out.println("Building Wooden Walls");
  }
  
  @Override
  public void buildPillars() {
    System.out.println("Building Pillars with Wood coating");
  }
}
```

<br>

### 템플릿 메소드 패턴의 장점

- 전체적으로는 동일하지만 부분적으로 중복되는 코드의 경우 상속을 활용해 중복을 최소화함
- 확장에 용이

<br>

#### [참고]

> - https://yaboong.github.io/design-pattern/2018/09/27/template-method-pattern/
> - https://niceman.tistory.com/142
> - https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html

