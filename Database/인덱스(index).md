# 인덱스(Index)

**인덱스(Index)는 RDBMS에서 검색속도를 높이기 위해 사용하는 하나의 기술이다**. 이름 그대로 색인(index)으로 특정 테이블의 컬럼을 색인화(따로 파일로 저장)하여 검색시 해당 테이블의 레코드를 full scan하는 것이 아니라 색인화 되어 있는 INDEX 파일을 검색하여 검색 속도를 빠르게 한다.

![](https://t2.daumcdn.net/thumb/R720x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/6tNj/image/f75XMzVB8zNcBq1j0toE7M0Nknc.png)

### 인덱스 저장 위치

초기 Table 생성시, {table명}.frm, {table명}.MYD, {table명}.MYI 3개의 파일이 만들어진다.

- **.frm** : 테이블 구조가 저장되어 있는 파일
- **.MYD**: 실제 데이터가 있는 파일
- **.MYI** : INDEX 정보가 들어있는 파일

인덱스를 사용하지 않는 경우, MYI파일은 비워져 있다. 인덱스를 해당 컬럼에 만들게 되면 해당 컬럼을 따로 인덱싱하여 MYI파일에 입력한다. 이후 사용자가 SELECT 쿼리로 인덱스를 사용하는 쿼리를 사용시 해당 Table을 검색하는 것이 아니라 MYI 파일의 내용을 검색한다.

<br>

### 인덱스의 자료구조

일반적으로 검색에 효율적인 Balance Search Tree를 사용한다. B+-Tree 알고리즘을 주로 사용한다.

`데이터에 접근하는 시간복잡도가 O(1)인 hash table이 더 효율적이지 않나?` - SELECT 질의 조건에는 부등호(<>) 연산도 포함된다. Hash table을 사용하게 되면 부등호 연산의 경우에 문제가 발생할 수 있기 때문에 적합하지 않다. (B+-Tree는 정렬된 상태를 유지하기 때문에 가능)

<Br>

### 인덱스의 장점

- 테이블에서 검색과 정렬 속도를 향상시킨다.
- 그 결과 시스템의 부하가 줄어들어 시스템 전체 성능 향상에 도움이 된다.

<br>

### 인덱스의 단점

- 인덱스가 데이터베이스 공간을 차지해 추가적인 공간이 필요해진다. (DB의 10퍼센트 내외의 공간이 추가로 필요)
- 인덱스를 생성하는데 시간이 많이 소요될 수 있다.
- 인덱스는 검색에 최적화된 기능이기 때문에, 삽입, 삭제, 수정이 자주 일어날 경우에 인덱스를 재작성해야 할 필요가 있기에 성능에 영향을 끼칠 수 있다.
  - **INSERT** : 인덱스 추가로 인해 index split이 발생하고 이는 성능면에서 불리하다.
    - Index split : 새로운 index key가 들어왔을 때 블록 내에 저장할 영역이 없어 새로운 블록을 할당하는 것
  - **DELETE** : index에서 데이터가 delete 되는 경우, 데이터가 지워지지 않고 사용 안됨 표시만 해두기 때문에 공간 낭비가 생긴다.
  - **UPDATE** : index에는 update라는 개념이 존재하지 않는다. delete와 insert 작업이 동시에 일어나기 때문에 큰 부하를 가져온다.

<br>

### 인덱스의 종류

인덱스의 종류에는 크게 2가지가 있다.

#### Clustered Index

- 테이블의 데이터를 지정된 컬럼에 대해 물리적으로 재배열
- 테이블 당 한개만 존재 (Primary key를 기준으로 자동으로 생성됨)

#### Non-Clustered Index

- 테이블의 데이터를 지정된 컬럼에 대해 물리적으로 재배열 안함
- 테이블 당 여러개 존재 가능
- 클러스터형 인덱스처럼 자동 정렬되지는 않음.

<br>

### [참고]

> - https://lalwr.blogspot.com/2016/02/db-index.html
> - https://k39335.tistory.com/26
> - https://brunch.co.kr/@skeks463/25
> - https://m.blog.naver.com/PostView.nhn?blogId=rladlaks123&logNo=220475359364&proxyReferer=https:%2F%2Fwww.google.com%2F
> - https://swconsulting.tistory.com/381
> - https://serverwizard.tistory.com/93

