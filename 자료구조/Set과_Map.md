# Set과 Map

![img](https://t1.daumcdn.net/cfile/tistory/2144004757FE1E332A)

### Set

- 순서를 유지하지 않는 데이터의 집합
- 데이터의 중복을 허용하지 않음
- 집합의 개념
- 비선형 구조이기 때문에 값을 추가/삭제 할 때 추가/삭제하고자 하는 값이 Set 내부에 있는지 검색한 뒤에 가능. 속도가 느림. `O(n)`

#### HashSet

- Set의 성질을 그대로 상속 (중복 허용 x, 순서 유지 x)
- HashSet이 중복을 걸러내는 과정
  - 객체를 저장하기 전에 먼저 객체의 hashCode()메소드를 호출하여 해시 코드를 얻어냄
  - 저장되어 있는 객체들의 해시 코드와 비교한 뒤 같은 해시 코드가 있는지 찾음
  - 같은 해시 코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단

#### TreeSet

- Set의 성질을 그대로 상속 (중복 허용 x, 순서 유지 x)
  - 이진 탐색 트리(Binary Search Tree) 구조로 이루어짐. => `레드-블랙 트리(Red-Black Tree)`
- HashSet과는 달리 정렬된 순서 유지
- 검색과 정렬에 유리함. `O(log n)`
- 객체 간의 비교를 위해 Comparator, Comparable 필요. 

**레드-블랙 트리(Red-Black Tree)** 

- 이진 탐색 트리 중에서도 성능을 향상시킨 자료구조.
  - 일반적인 이진 탐색 트리는 트리의 높이만큼 시간이 걸림.
  - 편향된 이진 탐색 트리의 경우 한쪽으로 크게 치우쳐져 있어서 비효율적이게 됨.
  - 트리의 균형을 맞추어 주는 알고리즘을 사용하여 해결. **자가 균형 이진 탐색트리(self-balancing binary search tree)**

#### LinkedHashSet

- Set의 성질대로 중복을 허용하지 않지만, 입력 순서를 보장

</Br>

### Map

- Key-Value 쌍으로 이루어진 데이터의 집합
- 순서는 유지되지 않음
- 키는 중복을 허용하지 않지만, 값은 중복을 허용한다.
- key를 알고 있다면 검색 시간은 `O(1)`. key를 모른다면 최악의 경우 `O(n)`

#### HashMap

- Map의 성질을 그대로 상속 (key-value 형태, 순서 유지 x, key 중복 허용 x)
- 보조 해시 함수(Additional Hash Function)를 사용하기 때문에 HashTable에 비하여 해시 충돌(hash collision)이 덜 발생할 수 있어 상대적으로 성능상 이점이 있음.
- JDK 1.2
- 지속적인 성능 개선이 이루어지고 있음

#### HashTable

- HashMap과 같은 동작
- JDK 1.0
- 동기화 지원(메소드 호출 전 쓰레드간 동기화 락을 통해 멀티 쓰레드 환경에서 data의 무결성을 보장)
  - 단일 쓰레드 환경에서도 동기화 작업이 이루어져 느림.
- 현재에는 기존 코드와의 호환성을 위해서만 남아있음. 
  - HashTable보다는 HashMap을 사용하는 것을 권장.
  - Java5부터는 동기화 지원이 필요하다면 ConcurrentHashMap를 쓰는 것을 권장한다고 함

#### TreeMap

- key와 value를 한 쌍으로 하는 데이터 집합
  - 이진 검색 트리(Binary search tree) 형태로 저장 => `레드-블랙 트리(Red-Black Tree)`

#### LinkedHashMap

- HashMap을 상속받아 HashMap과 흡사
- 입력 순서를 보장

</br>

#### 해시(Hash)란?

해시 함수란 임의 크기 데이터를 고정 크기 값으로 매핑하는 데 사용할 수 있는 함수를 말한다. 그리고 데이터를 인덱싱하기 위해 해시 함수를 사용하는 것을 해싱(Hashing)이라고 한다. 

![img](https://t1.daumcdn.net/cfile/tistory/25758F43580F530822)



#### 해시 충돌(Hash Collision)

해시 함수는 해쉬 값의 개수보다 대개 많은 키 값을 해쉬 값으로 변환(many-to-one 대응)하기 때문에 해시 함수가 서로 다른 두 개의 키에 대해 동일한 해시값을 내는 해시 충돌(collision)이 발생한다.

![해시 함수 - 위키백과, 우리 모두의 백과사전](https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Hash_table_4_1_1_0_0_1_0_LL.svg/1200px-Hash_table_4_1_1_0_0_1_0_LL.svg.png)

해시 충돌을 해결하기 위한 방법에는 2가지가 존재한다. 

**1. 개별 체이닝(Seperate Chaining)**

![img](https://t1.daumcdn.net/cfile/tistory/2525963E580F616926)

- 충돌 발생 시 연결리스트로 연결하는 방식.
- 모든 데이터가 해시 충돌이 일어나는 최악의 경우, 데이터가 연결 리스트로 이루어지게 되므로 시간복잡도가  `O(n)`이 될 수 있다.
- Java8 버전에서는 성능을 향상 시키기 위해 충돌된 데이터가 8개가 모이면 레드 블랙 트리로 변환하여 탐색에 유리하도록 한다.
- 장점 :
  - 한정된 저장소(Bucket)을 효율적으로 사용할 수 있다.
  - 해시 함수(Hash Function)을 선택하는 중요성이 상대적으로 적다.
  -  ~~상대적으로 적은 메모리를 사용한다.~~ 미리 공간을 잡아 놓을 필요가 없다.
- 단점 :
  - 한 Hash에 자료들이 계속 연결된다면(쏠림 현상) 검색 효율을 낮출 수 있다.
  - 외부 저장 공간을 사용한다.
  - 외부 저장 공간 작업을 추가로 해야 한다.

**2. 오픈 어드레싱(Open Addressing)**

![C++, Java 해시 테이블 구현 · The Missing Papers](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Hash_table_5_0_1_1_1_1_0_SP.svg/450px-Hash_table_5_0_1_1_1_1_0_SP.svg.png)

- 충돌 발생 시 탐사(probing)을 통해 빈 공간을 찾아나서는 방식.
  - 선형 탐색(Linear Probing): 해시충돌 시 다음 버켓, 혹은 몇 개를 건너뛰어 데이터를 삽입한다.
  - 제곱 탐색(Quadratic Probing): 해시충돌 시 제곱만큼 건너뛴 버켓에 데이터를 삽입(1,4,9,16..)
  - 이중 해시(Double Hashing): 해시충돌 시 다른 해시함수를 한 번 더 적용한 결과를 이용함.
- 빈 공간을 찾아 값을 저장하기 때문에 모든 원소가 반드시 자신의 해시 값과 일치하는 주소에 저장된다는 보장은 없음.

- 장점 :
  - 또 다른 저장공간 없이 해시테이블 내에서 데이터 저장 및 처리가 가능하다.
  - 또 다른 저장공간에서의 추가적인 작업이 없다.

- 단점 :
  - 해시 함수(Hash Function)의 성능에 전체 해시테이블의 성능이 좌지우지된다.
  - 데이터의 길이가 늘어나면 그에 해당하는 저장소를 마련해 두어야 한다.

</br>

#### [참고]

> - https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o
> - https://preamtree.tistory.com/20
> - https://d2.naver.com/helloworld/831311
> - https://odol87.tistory.com/4
> - http://www.tcpschool.com/java/java_collectionFramework_map
> - https://limkydev.tistory.com/40
> - https://lelumiere.tistory.com/3
> - https://pridiot.tistory.com/72
> - https://coding-factory.tistory.com/554
> - https://coding-factory.tistory.com/555
> - https://withwani.tistory.com/150