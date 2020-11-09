# 웹 앱개발 - SpringMVC 

## SpringMVC

### MVC(Model-View-Controller)

- Model : 뷰가 렌더링하는데 필요한 데이터, 예를 들어 사용자가 요청한 상품 목록이나, 주문 내역 등
- View : 실제로 보이는 부분, 모델을 사용해 렌더링한다. JSP, JSF, PDF, XML등으로 결과를 표현
- Controller : 사용자의 액션에 응답하는 컴포넌트로, 모델을 업데이트하고, 다른 액션을 수행한다.

##### 

## SpringMVC 구성요소

### DispatcherServlet

- 프론트 컨트롤러 (Front Controller)
- 클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.
- DispathcerServlet은 여러 컴포넌트를 이용해 작업을 처리한다.





참고: [부스트코스](https://www.edwith.org/boostcourse-web/lecture/16763/)