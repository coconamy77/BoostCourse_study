# 웹 프로그래밍 - Web API

### API(Application Programming Interface)

- 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

## Rest API(REpresentational State Transfe API)

- 핵심 컨텐츠 및 기능을 외부 사이트에서 활용할 수 있도록 제공되는 인터페이스
- REST는 다음과 같은 스타일(제약조건)을 반드시 지켜야한다
  - client-server
  - stateless
  - cache
  - uniform interface
  - layered system
  - code-on-demand (optional)
- HTTP프로토콜을 사용한다면 client-server, stateless, cache, lared system, code-on-demand 등에 대해서는 모두 쉽게 구현 가능
- uniform interface
  - 리소스가 URI로 식별되야 한다.
  - 리소스를 생성,수정,추가하고자 할 때 HTTP메시지에 표현을 해서 전송해야 한다.
  - 메시지는 스스로 설명할 수 있어야 한다. (Self-descriptive message)
  - 애플리케이션의 상태는 Hyperlink를 이용해 전이되야 한다.(HATEOAS)



# Web API

- URI는 정보의 자원을 표현해야 한다.
- 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

###  HTTP Method

- POST
  - 리소스를 생성한다.
- GET
  - 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
- PUT
  - 리소스를 수정한다.
- DELETE
  - 리소스를 삭제한다.

### URI는 정보의 자원을 표현

- GET /members
  - 맴버의 모든 정보를 달라는 요청
- DELETE /members/1
  -  HTTP Method 중의 하나인 DELETE를 이용하여 삭제를 표현

- GET /members/1          (o)
- POST /members            (o)
- PUT /members/1           (o)

- 슬래시 구분자(/)는 계층을 나타낼 때 사용



### 상태 코드

- 성공
  - 200 : 정상적으로 수행
  - 201 : 클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨
- 클라이언트로 인한 오류
  - 400 : 요청이 부적절
  - 401 : 인증되지 않은 상태
  - 403 : 응답하고 싶지 않은 리소스 요청시 
  - 404 : 존재하지 않는 리소스
  - 405 : 요청 리소스에서 사용불가능한 Method
- 서버로 인한 오류
  - 301 : URI가 변경되었을 시, Location header에 변경된 URI를 함께 응답
  - 500 : 서버에 문제가 있을 경우





















참고: [부스트캠프](https://www.edwith.org/boostcourse-web/lecture/16740/)