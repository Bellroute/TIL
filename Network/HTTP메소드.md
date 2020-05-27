# HTTP 메소드

### HTTP 메소드

- 클라이언트와 서버 사이에서 이루어지는 요청(Request)와 응답(Response) 데이터를 전송하는 방식
- 클라이언트가 웹 서버에게 사용자 요청의 목적/종류를 알리는 수단
- 서버가 URL의 일부로 던져진 데이터를 갖고 어떤 일을 해야할지 알려줌



#### GET

- 서버의 Resource(데이터)를 요청
- 요청을 파라미터가 URL에 포함되어 있음 (qurey string)
  - URL 뒤에 '?'를 구분자로 파라미터를 입력
  - '&' 를 이용해 파라미터를 구분
  - 예시) id = 1, password =1234를 파라미터로 담는 경우 => http://www.test.com?id=1&password=1234

#### HEAD

- GET과 유사한 방식이지만, HTTP 응답 메시지에 Body 없이 HTTP Header 정보만을 보냄
- Resoruce 를 받지 않고 오직 찾기만 원할 때
- object가 존재할 경우 응답의 상태 코드를 확인할 때
- 해당 Resource가 변경되었는지 여부만 확인할 때

#### POST

- 서버에 Resource를 생성
- 요청을 여러 번 하면 여러 개의 Resource가 각각 만들어진다.

####PUT

- 서버에 Resource를 생성 및 변경
- 여러 차례 호출해도 동일한 리소스를 변경한다.

#### DELETE

- 요청 Resource를 삭제하도록 요청
- 하지만 DELETE의 경우 서버에서 클라이언트의 요청을 무시할 수 있음

#### CONNECT

- 프록시 서버와 같은 중간 서버 경유
- 요청한 리소스에 대해 양방향 연결을 시작하는 메소드
- SSL(HTTPS)를 사용하는 웹 사이트를 접속하는데 사용 가능
  - 클라이언트는 원하는 목적지와 TCP연결을 HTTP 프록시 서버에 요청
  - 서버는 클라이언트를 대신하여 연결의 생성을 진행
  - 한번 서버에 의해 연결이 수립되면, 프록시 서버는 클라이언트에 오고가는 TCP 스트림을 계속해서 프록시함

#### OPTIONS

- 요청한 URL에 대한 웹 서버 측 제공 메소드에 대한 질의
- HTTP 응답 헤더에 'Allow: GET, POST, HEAD'로 보내게 된다.

#### TRACE

- 요청 리소스가 수신되는 경로를 보여줌
- 원격지 서버에 Loopback(루프백) 메시지를 호출하기 위해 사용
- 크로스 사이트 트레이싱(XST)와 같은 공격을 일으키는 보안상 문제로 인해 거의 사용되지 않음

#### PATCH

- PUT과 유사하게 Resource를 변경
- Resource의 부분만 교체



### HTTP 메소드 차이점 비교

#### GET vs POST

GET은 요청 메시지에 body를 포함하지 않는다. 또한 요청에 필요한 파라미터를 쿼리 스트링의 형태로 URL 뒤에 이어 붙인다. 반면, POST는 body에 데이터를 담아서 요청을 보낸다. 외부에 노출되면 안되는 정보라면 POST를 사용하여 body에 데이터를 담는 것이 좋다. 멱등성 측면에서 비교를 한다면, GET은 서버에 동일한 요청을 여러 번 전송하더라도 동일한 응답이 돌아오지만 POST는 같은 요청을 여러 번 하더라도 응답이 다를 수 있다.

\* `멱등성(Idempotent)`: 같은 요청을 여러번 수행해도 결과가 나타타는 성질



#### POST vs PUT

POST는 Create, PUT은 update의 의미로 사용하지만 PUT도 POST와 마찬가지로 자원을 생성 가능하다. 가장 큰 차이는 멱등성이라고 할 수 있다.  POST의 경우 같은 요청을 여러 번 수행하면 수행한만큼 자원이 생성되기 때문에 멱등하지 않다. 반면 PUT은 여러 번 요청을 수행해도 하나의 자원이 생성&수정되기 때문에 멱등하다.



#### PUT vs PATCH

PUT은 Resource의 모든 필드를 교체한다. 요청 시에 일부만 전달할 경우 전달한 필드 외 모두 null or 초기값 처리될 수 있다. 반면 PATCH는 자원의 부분만 교체한다. 요청 시에 일부만 전달할 경우 해당 필드만 수정된다.



### 참고

- [6. HTTP 메소드](https://blog.sonim1.com/95)
- [HTTP Method](https://medium.com/@lyhlg0201/http-method-d561b77df7)
- [HTTP 요청 메소드 정리](https://zorba91.tistory.com/136)
- [RESTful API](https://lifeisgift.tistory.com/entry/Restful-API-%EA%B0%9C%EC%9A%94)
- [[HTTP METHOD] PUT vs PATCH 차이점](https://papababo.tistory.com/269)

