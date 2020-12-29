# 웹 프로그래밍 -  **redirect & forward**

## redirect

### 리다이렉트(302)

- HTTP프로토콜로 정해진 규칙

- 서버는 클라이언트의 요청에 대해 특정 URL로 이동을 요청할 수 있다.(=리다이렉트)

- 서버는 클라이언트에게 HTTP 상태코드 302로 응답, 이때 헤더 내 Location 값에 이동할 URL 을 추가

   -> 클라이언트는 서버로부터 받은 상태 값이 302이면 Location헤더값으로 재요청, 이때 브라우저의 주소창은 전송받은 URL로 바뀌게 된다.

- 서블릿이나 JSP는 리다이렉트하기 위해 HttpServletResponse 클래스의 sendRedirect() 메소드를 사용

```java
response.sendRedirect("redirect02.jsp");
```



### 특징!

- 리다이렉트를 사용할 경우, 클라이언트는 항상 요청을 두번 보내게 된다.
- 처음 요청시의 요청, 응답 객체와 리다이렉트시의 요청, 응답 객체는 다르다!



## forward

### forward

- {Client} ----(request)--->Servlet 1---(forward)--->Servlet 2---(response)--->{Client}
- 요청받은 서버가 혼자서 다 처리 하지않고 다른 서버(servlet)에게 넘겨서 마저 처리하도록 보내는 것

```java
RequestDispatcher requestDispatehcer = request.getRequestDispatcher(url);
requestDispatehcer.forward(request, response);
```



### redirect & forward

- r: 클라이언트가 두번 요청을 보내도록 하는 것, request와 response가 두개씩 생성된다.
- f: 클라이언트는 한번 요청을 보내고 서버에서 다른 서버에게 처리를 부탁한 후 응답을 보내는 것, request와 response가 각각 하나씩 계속 유지된다.



## servlet & jsp 연동

- servlet은 로직을 처리하기에 좋고, jsp는 html을 처리하기에 좋다.
- Client ---(LogicServlet 요청)---> LogicServlet ---(forward)--->result.jsp---(result.jsp결과 출력)--->Client

```java
RequestDispatcher requestDispatcher = request.getRequestDispatcher("/result.jsp");
requestDispatcher.forward(request, response);
```









참고 : [부스트캠프](https://www.edwith.org/boostcourse-web/lecture/16706/)