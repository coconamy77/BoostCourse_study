# 웹 프로그래밍 - JSP
## JSP(Java Server Page)란?
- JSP는 JSP자체가 동작하는 것이 아니라, 모든 JSP는 서블릿으로 바뀌어서 동작
	- 톰캣이 JSP를 서블릿으로 변경
- html 문서 안에서 java 언어를 사용할 수 있다.
- <% ->지시자
- <%@ page ->page 지시문
- <%= ->응답결과

## JSP 라이프싸이클
### hello.jsp가 실행될 때, 
	- .metadata 폴더에 hello.java파일 생성
	- 해당 파일의 _jspService() 메소드 안에 jsp파일의 내용이 변환되어 들어가 있다.
	- hello.java는 서블릿 소스로 자동으로 컴파일 되면서 실행돼서 그 결과가 브라우저에 보여진다.

### JSP의 실행순서 
1. 브라우저가 웹서버에 JSP에 대한 요청 정보를 전달한다.
2. 브라우저가 요청한 JSP가 최초로 요청했을 경우만 
	1. JSP로 작성된 코드가 서블릿으로 코드로 변환한다. (java 파일 생성)
	2. 서블릿 코드를 컴파일해서 실행가능한 bytecode로 변환한다. (class 파일 생성)
	3. 서블릿 클래스를 로딩하고 인스턴스를 생성한다.
3. 서블릿이 실행되어 요청을 처리하고 응답 정보를 생성한다.

### 항상 Service안에서만 사용? No
- <%! ->선언식: service 바깥쪽에 코드들이 만들어진다.

##  JSP 문법
### 스크립트 요소의 이해
- 선언문(Declaration) - <%! 문장 %> : 전역변수 선언 및 메소드 선언에 사용
	- 여기에 선언하는 문장은 서비스 메서드가 아니라 클래스 바디 쪽에 해당 코드가 바뀐다.
- 스크립트릿(Scriptlet) - <% 문장 %> : 프로그래밍 코드 기술에 사용
	- 가장 일반적으로 많이 쓰인다.
	- 지역변수
- 표현식(Expression) - <%= %> : 화면에 출력할 내용 기술에 사용. 즉, 화면에 출력하기 위한 것
	- 스크립트릿내에서 출력할 부분은 내장객체인 out객체의 print()또는 println()메소드 사용
- 주석(Comment)
	- HTML주석 : <!-- -->
	- JSP 주석 : <%-- JSP 주석입니다. --%>, Java로 변환되지 않는다.
	- Java 주석 : jsp가 java로 변환될때 변한다, 하지만 java->servlet 이때 실행 x

## JSP내장객체
- response, request, application, session, out과 같은 변수를 내장객체
- JSP안에서 자유롭게 사용할 수 있다.



참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16703/)