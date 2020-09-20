# 웹 프로그래밍 - Servlet
## Servlet 라이프 싸이클
### 서블릿 생명주기
- HttpServlet의 3가지 메소드
	- init()  ->WAS는 서블릿 요청을 받고, 해당 서블릿이 메모에 있는지 확인후, 생성
	- service(request, response)
	- destroy()
	- 생성 -> init -> service -> destroy
- 단, 새로고침/브라우저 내에서 재접속 시 service만 새로 호출하는데, 서블릿 객체는 한 서버 내에서 한번만 생성되기 때문이다.

### service(request, response)메소드
- 클라이언트의 요청(GET/POST)에 따라 가지고 있는 메소드(doGet()/doPost())를 호출


참고: [부스트캠프](https://www.edwith.org/boostcourse-web/lecture/16688/)                                                                                     