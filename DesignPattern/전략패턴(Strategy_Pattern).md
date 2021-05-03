# 전략 패턴(Strategy Pattern)

특정 컨텍스트(Context)에서 알고리즘(전략)을 별도로 분리하는 설계 방법

![img](https://blog.kakaocdn.net/dn/5cvPZ/btqL2wNUAt3/h3XLVqlZ5dg2XBYhKfSfsK/img.png)

- 실행 중에 알고리즘을 선택할 수 있게 하는 행위 소프트웨어 디자인 패턴 - 위키 백과
- 특정한 계열의 알고리즘들을 정의하고 각 알고리즘을 캡슐화하며 이 알고리즘들을 해당 계열 안에서 상호 교체가 가능하게 만든다.알고리즘을 사용하는 클라이언트와 상관없이 독립적으로 알고리즘을 다양하게 변경할 수 있게 한다.  - GoF

특정 기능이 추가될 때, 전략 객체를 생성 후 주입하는 방식으로 유연하게 기능을 확장할 수 있다.

<br>

### 전략 패턴 예시

#### 상황

- 상황에 따라 다른 가격 할인 정책을 적용하는 과일 매장
- 제일 먼저 온 손님에게 10% 할인
- 마지막 손님은 20% 할인
- 신선도가 떨어진 과일의 경우 20% 할인

#### 전략 패턴이 적용되지 않은 코드

```java
    public class Calculator {
    
    	public double calculate(boolean isFirstGuest, boolean isLastGuest, List<Item> items) {
    		double sum = 0;
    		for (Item item : items) {
    			if (isFirstGuest) {
    				sum += item.getPrice() * 0.9;
    			} else if (!item.isFresh()) {
    				sum += item.getPrice() * 0.8;
    			} else if (isFirstGuest) {
    				sum += item.getPrice() * 0.8;
    			} else {
    				sum += item.getPrice();
    			}
    		}
    		return sum;
    	}
    }
    
    public class Item {
    	private final String name;
    	private final int price;
    
    	public Item(String name, int price) {
    		this.name = name;
    		this.price = price;
    	}
    
    	public int getPrice() {
    		return price;
    	}
    
    	public boolean isFresh() {
    		return true;
    	}
    }
```

- 특정 할인 정책을 사용하기 위해서는 할인 조건이 충족하는지를 if-else 분기를 타며 해결하고 있음.
- 하나의 메소드에 너무 많은 확인 로직이 추가되고, 정책이 추가될 때마다 분기가 늘어남. 변경에 유연하지 않음.

#### 전략 패턴이 적용된 코드

전략 패턴은 특정 컨텍스트에 다양한 알고리즘을 별도로 분리해 관리한다고 했음. 여기서 계산을 컨텍스트로 보고 각 할인 정책들을 별도로 분리해 관리함으로써 전략 패턴을 적용해볼 수 있다.

할인 정책이라는 알고리즘을 `DiscountPolicy`라는 인터페이스를 통해 다음과 같이 분리한다.

```java
 public interface DiscountPolicy {
    	double calculateWithDisCountRate(Item item);
    }
    
		// 첫 번째 손님에 대한 할인 정책
    public class FirstCustomerDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.9;
    	}
    }
    
		// 마지막 손님에 대한 할인 정책
    public class LastCustomerDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.8;
    	}
    }
    
    // 신선하지 않은 과일에 대한 할인 정책
    public class UnFreshFruitDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.8;
    	}
    }
```

그리고 이를 기존의 `Calculator` 클래스에서 setter 메소드를 통해 필요한 하위 타입을 주입받는다.

```java
    public class Calculator {
    
    	private final DiscountPolicy discountPolicy;
      
      public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
      }
    
    	public double calculate(List<Item> items) {
    		double sum = 0;
    		for (Item item : items) {
    			sum += discountPolicy.calculateWithDisCountRate(item);
    		}
    		return sum;
    	}
    }
```

다음과 같이 필요에 따라 다른 할인 정책 클래스를 주입하는 것이 가능하다.

```java
    public class Main {
    	public static void main(String[] args) {
    		Calculator calculator = new Calculator();
        
        // 첫 번째 손님에 대한 할인 정책 적용
        calculator.setDiscountPolicy(new FirstCustomerDiscount());
    		calculator.calculate(Arrays.asList(
    			new Item("Apple", 3000),
    			new Item("Banana", 3000),
    			new Item("Orange", 2000),
    			new Item("Pitch", 4000)
    		));
        
        // 마지막 손님에 대한 할인 정책 적용
        calculator.setDiscountPolicy(new LastCustomerDiscount());
    		calculator.calculate(Arrays.asList(
    			new Item("Apple", 3000),
    			new Item("Banana", 3000),
    			new Item("Orange", 2000),
    			new Item("Pitch", 4000)
    		));
        
        // 신선하지 않은 과일에 대한 할인 정책 적용
        calculator.setDiscountPolicy(new UnFreshFruitDiscount());
    		calculator.calculate(Arrays.asList(
    			new Item("Apple", 3000),
    			new Item("Banana", 3000),
    			new Item("Orange", 2000),
    			new Item("Pitch", 4000)
    		));
    	}
    }
```

<br>

### 전략 패턴의 장점

- 요구 사항이 변경되었을 때 기존의 코드 변경 없이 새로운 전략을 추가할 수 있다.
- 새로운 전략에 대해서는 새로운 클래스를 통해 관리하기 때문에 OCP의 원칙을 준수할 수 있다.

<br>

### 전략 패턴의 단점

- 컨텍스트에 적용되는 알고리즘이 하나이거나 두개인 경우는 분기를 타는 것이 더 편하고, 가독성이 좋을 수 있다.

<br>

#### Q. 전략 패턴 사용해 본 경험있나?

Spring Security와 spring-boot-starter-oauth2 를 사용하게 되서 구현 도중에 중단했었지만, 프로젝트에서 로그인 및 회원가입 기능을 구현할 때 이메일을 통한 로그인과 소셜서비스를 통한 로그인 로직을 구현할 때 시도했던 것이 이제 생각해보니 전략 패턴이었던 것 같다. 특히, 로그인 가능한 소셜서비스로 github, facebook, google이 있었는데 다른 팀원이 if-else문으로 분기했던 코드를 리팩토링하면서 각각의 소셜 로그인 로직을 클래스로 분리해서 구현했었다.

<br>

#### [참고]

> - https://velog.io/@kyle/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%84%EB%9E%B5%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80
> - https://bamdule.tistory.com/164
> - https://rok93.tistory.com/entry/1-%EC%A0%84%EB%9E%B5Strategy-%ED%8C%A8%ED%84%B4

