# REST API

### API(Application Programming Interface)

- 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

- 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단



## REST API(REpresentational State Transfe API)

- 요청이 어떤 정보, 어떤 요청인지 요청의 모습으로 추론이 가능하다!!(RESTful한 API)
  - 보통 URI의 구성은 동사가 아닌 명사로 이루어져 있다.

#### Method

- POST, PUT, PATCH는 body를 가지고 있어서 GET, DELETE보다 많은 정보를 안전하게 보낼 수 있다.
- GET : read, 데이터 조회
- POST : create, 새로운 정보 추가
- PUT : update, 기존 정보를 변경
- PATCH : update, 기존 정보의 일부분을 특별한 방식으로 변경
- DELETE : delete, 기존 정보 삭제