# 제네릭(Generics)과 컬렉션(Collection)

### 컬렉션(Collection)?

> 배열이 가진 고정 크기의 단점을 극복하기 위해 객체들을 쉽게 삽입, 삭제, 검색할 수 있는 가변 크기의 컨테이너(Container)

자바 초기에는 Vector, Stack, HashTable 등의 컬렉션 클래스만 제공했으나, 자바 1.2 이후 표준적인 방식으로 컬렉션을 다루기 위한 **컬렉션 프레임워크(Collection Framework)**가 등장하였다. **java.util** 패키지에서 다양한 컬렉션 인터페이스와 컬렉션 클래스를 제공한다.



### 컬렉션 프레임워크의 장점

- 별도로 컬렉션 클래스를 구현하는 것보다 이미 구현되어 있는 것을 사용함으로써 **코딩 시간을 감소**시킬 수 있다. 
- 컬렉션 프레임워크들은 잘 테스트되고 검증되어 있기 때문에 **코드 품질을 보장**한다.
-  JDK에 포함된 컬렉션 프레임워크들을 사용하여 **코드 유지보수 시간을 감소**시킬 수 있다. 
- **재사용 가능**하고 **상호 운용성이 보장**된다.



### 제네릭(Generics)

컬렉션은 제네릭(generics)이라는 기법으로 구현되어 있다. 제네릭은 JDK 1.5 버전부터 도입되었다. 제네릭은 모든 종류의 타입을 다룰 수 있도록, 클래스나 메소드를 **타입 매개변수(generic type)**를 이용하여 선언하는 기법이다. 제네릭은 클래스 코드를 찍어내듯이 생산할 수 있도록 **일반화(generic)** 시키는 도구이다.

컬렉션 클래스에서 타입 매개변수로 사용하는 문자는 다른 변수와 혼동을 피하기 위해 일반적으로 하나의 대문자를 사용한다.

- **E** : Element를 의미하며 컬렉션에서 요소임을 나타냄
- **T** : Type을 의미
- **V** : Value를 의미
- **K** : Key를 의미



### 컬렉션(Collection)의 특징

1. **컬렉션 클래스 이름에는 \<E>, \<K>, \<V> 등이 항상 표시된다.**

   Vecgtor\<E>에서 E 대신 Integer와 같이 구체적인 타입을 지정하면 Vector\<Integer>는 정수 값만 사용하는 벡터로 사용 가능하다. 특정 타입만 다루지 않고 여러 종류의 타입으로 변신할 수 있도곡, 컬렉션을 일반화 시키기 위해 사용하는 것이다.

2. **컬렉션의 요소는 객체들만 가능하다.**

   Int, char, double 등의 기본 타입의 데이터는 원칙적으로 컬렉션의 요소로 불가능하다. 때문에 Integer, Charater, Double 등 Wrapper 클래스로 객체화하여 삽입해야 한다.

   \* `Wrapper 클래스` : 기본자료형을 객체자료형으로 변환하기 위해서 사용하는 클래스



### 컬렉션 프레임워크

![img](https://t1.daumcdn.net/cfile/tistory/2614AF3655269C1129)

다음은 자바 컬렉션 프레임워크의 상속 구조이다. 사용 용도에 따라 List, Set, Map 3가지로 요약할 수 있다.



| **인터페이스** | **구현 클래스**                                  | **특징**                                                     |
| -------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| **List**       | LinkedList<br />Stack<br />Vector<br />ArrayList | 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.         |
| **Set**        | HashSet<br />TreeSet                             | 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다. |
| **Map**        | HashMap<br />TreeMap<br />HashTable              | 키(key)와 값(value)의 쌍으로 이루어진 데이터의 집합이다. 순서는 유지되지 않고, 키는 중복을 허용하지 않으며 값의 중복을 허용한다. |



#### 0. Collection 인터페이스

모든 컬렌션의 상위 인터페이스. 직접적인 구현은 제공하지 않으며 모든 컬렉션 클래스가 구현해야 하는 메서드들을 포함하고 있다. 아래는 대표적인 메서드들이다.

- boolean add(E e) : 해당 컬렉션에 전달된 요소를 추가
- boolean remove(Object o) : 해당 컬렉션에서 전달된 객체를 제거
- void clear() : 해당 컬렉션의 모든 요소를 제거
- boolean contains(Object o) : 해당 컬렉션이 전달된 객체를 포함하고 있는지
- boolean equals(Object o) : 해당 컬렉션과 전달된 객체가 같은지
- boolean isEmpty() : 해당 컬렉션이 비어있는지
- Iterator \<E> iterator() : 해당 컬렉션의 반복자(iterator)를 반환
- int size() : 해당 컬렉션의 요소의 총개수를 반환
- Object [] toArray() : 해당 컬렉션의 모든 요소를 Object 타입의 배열로 반환



#### 1. List 인터페이스

Collection 인터페이스를 상속. **순서**가 있는 컬렉션이며, 색인(index)을 이용하여 특정 위치에 요소를 삽입하거나 조회 가능하다. **중복을 허용**한다.

##### ArrayList

- 속도가 빠르고(멀티스레드 동기화를 위한 시간 소모가 없기 때문) 크기를 마음대로 조절할 수 있는 배열
- 단방향 포인터 구조로 자료에 대한 순차적인 접근에 강점이 있음
- 스레드 간 동기화를 지원하지 않음 -> 다수의 스레드가 동시에 ArrayList에 접근할 때 데이터가 훼손될 우려가 있음.
- 요소의 삽입이나 삭제 시 중간에 빈 공간이 생기지 않도록 요소들의 위치를 앞뒤로 이동

##### Vector

- ArrayList의 구형버전이며, 모든 메소드가 동기화 되어있음
- 요소의 삽입이나 삭제 시 중간에 빈 공간이 생기지 않도록 요소들의 위치를 앞뒤로 이동

##### LinkedList

- 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번한 경우 빠른 성능을 보장
-  Queue, Deque 속성과 메서드를 가지고 있고 비동기이다.
- ArrayList 클래스의 주요 메서드뿐 만 아니라 Queue, Deque의 메서드도 포함

##### 

#### 2. Set 인터페이스

**중복 요소를 포함할 수 없으며** 랜덤 액세스(Random access)를 허용하지 않으므로, iterator 또는 foreach를 이용하여 요소를 탐색할 수 있다.

##### HashSet

- set의 반복 순서를 보장하지 않으며 null 요소를 허용한다.
- 가장 빠른 임의 접근 속도

##### TreeSet

- 정렬된 순서로 보관하며 정렬방법을 지정할 수 있음
- 요소를 오름차순으로 유지하는 SortedSet 인터페이스의 구현체

##### LinkedHashSet

- 추가된 순서, 또는 가장 최근에 접근한 순서대로 접근 가능



#### 3. Map 인터페이스

**키와 값을 매핑**한다. 중복 키가 존재할 수 없으며 각 키는 하나의 값만 매핑할 수 있다. Map의 기본 연산은 put, get, containsKey, containsValue, size, isEmpty 등이 있다.

##### HashMap

- 중복을 허용하지 않고 순서를 보장하지 않음
- 키와 값으로 null 허용
- 동기화 보장 x

##### HashTable

- HashMap과 동일
- null을 허용하지 않음
- 동기화 보장

##### TreeMap

- 이진탐색트리(binary search tree) 형태로 키와 값의 쌍으로 이루어진 데이터를 저장 -> 빠르게 데이터 접근 가능
- key를 기준으로 정렬되어 있음



### 참고

> - 명품 JAVA Programming - 제 7장 제네릭과 컬렉션
> - [자바 컬렉션 프레임워크(Java Collection Framework) 정리](https://gbsb.tistory.com/247)
> - [[Java] Wrapper 클래스](https://devbox.tistory.com/entry/Java-Wrapper-클래스)
> - [Java의 Collections (List, Set, Map) 이해](https://hackersstudy.tistory.com/26)
> - [HashMap, TreeMap 그리고 LinkedHashMap의 차이]( https://tomining.tistory.com/168)