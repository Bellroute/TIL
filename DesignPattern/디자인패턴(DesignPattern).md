# 디자인 패턴(Design Pattern)

### 디자인 패턴이란?

소프트웨어를 설계할 때 특정 맥락에서 자주 발생하는 고질적인 문제들이 또 발생했을 때 재사용할 할 수있는 훌륭한 해결책

- 일반 프로그래머가 만나는 문제가 지구상에서 유일한 문제)일 확률은 거의 없다. 이미 수많은 사람들이 부딪힌 문제다. 따라서 전문가들이 기존에 해결책을 다 마련해 놓았다.

#### 디자인 패턴 사용 이유

“바퀴를 다시 발명하지 마라(Don’t reinvent the wheel)” 이미 만들어져서 잘 되는 것을 처음부터 다시 만들 필요가 없다는 의미이다.

- 요구 사항에 따른 설계 변경에 대한 유연한 대처가 가능

  - 상황에 맞는 올바른 설계를 더 빠르게 적용할 수 있다. 

    각 패턴의 장단점을 통해서 설계를 선택하는데 도움을 얻을 수 있다.

- 여러 사람이 협업해서 개발 할 때 범용적인 코딩 스타일을 적용 가능

  -  “기능마다 별도의 클래스를 만들고, 그 기능들로 해야할 일을 한번에 처리해주는 클래스를 만들자.”라고 제안하는 것보다 "Facade 패턴을 써보자."라고 제안 => 원활한 의사소통 가능
  - 재사용을 통한 개발 시간 단축

- 소프트웨어 구조 파악 용이

#### 디자인 패턴의 단점

- 객체 지향 위주의 설계 및 구현에 사용됨
- 초기 투자 비용이 부담
- 디자인 패턴을 맹신한 해결 방법 모색으로 오는 복잡성 => 디자인 패턴보다 중요한 것은 코드베이스의 간결성. 패턴 자체에 얽매이는 것보단 그 패턴이 왜 효율적인지를 이해해야함

<br>

### 디자인 패턴 종류

GoF(Gang of Four) 디자인 패턴이라고 불리며, 4명의 유명한 개발자들에 의해 고안되었다. 

>  `GoF(Gang of Fout)` : 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides) => 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고 체계화한 사람들

23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.

#### 생성(Creational) 패턴

- 객체 생성에 관련된 패턴
- 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성 제공

#### 구조(Structural) 패턴

- 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
- 예를 들어 서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴

#### 행위(Behavioral) 패턴

- 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
- 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둠.

![img](https://gmlwjd9405.github.io/images/design-pattern/types-of-designpattern.png)

<br>

#### [참고]

> - https://namu.wiki/w/%EB%94%94%EC%9E%90%EC%9D%B8%20%ED%8C%A8%ED%84%B4
> - https://codedragon.tistory.com/8988
> - https://gmlwjd9405.github.io/2018/07/06/design-pattern.html
> - https://dailyheumsi.tistory.com/148
> - https://rok93.tistory.com/entry/1-%EC%A0%84%EB%9E%B5Strategy-%ED%8C%A8%ED%84%B4

<br>

