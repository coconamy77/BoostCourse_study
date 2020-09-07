# Spring과 Spring Boot

### Spring

- DI(Dependency Injection) : 의존성 주입

  - 객체를 직접 생성하는 것이 아닌, 외부에서 생성 후, 주입(setter()나 생성자를 이용)을 시켜주는 것

    - 객체를 직접 생성했을 때, 의존성이 높아진다. -> 강결합 -> 재사용이 용이하지 않다.
    - ex) 

    ```java
    class Fruit{
        Apple a = new Apple();
    }
    ```

    -> a를 생성한 Fruit클래스는 Apple 객체에 의존한다.==Fruit를 사용하기 위해서는 Apple이 필요하다.

    -> 스프링은 setter(), 생성자를 이용하여 의존성 객체를 주입시켜준다.

  - 객체를 Bean이라고 부르며, 프로젝트가 실행될 때 자동으로 객체를 생성해주는데, 객체 요청이 들어오면 스프링은 실행될 때 생성된 Bean을 주입시겨준다-> DI과정

- IoC(Inversion of Control) :  제어의 역전, 제어의 흐름을 바꾸는 것

  - 스프링이 실행 시 모든 의존성 객체를 만들어 주고 필요한 곳에 주입시켜주며 Bean들은 싱글턴 패턴의 특징을 가진다.
  - 제어의 흐름을 개발자가 컨트롤 하는 것이 아닌, 스프링에게 맡겨 작업을 처리하게 된다.

- 즉, DI와 IoC를 활용하여 개발자는 어플리케이션의 전체 흐름에 관여하지 않고 비즈니스 로직에만 집중할 수 있다. == 느슨하게 결합된 애플리케이션을 개발할 수 있다.

- Spring이 제공하는 Spring모듈
  - Spring JDBC
  - Spring AOP
  - Spring MVC
  - Spring ORM
  - Spring Test



### Spring Boot

- Spring의 복잡한 과정을 자동화 해주어 사용자가 편리하게 Spring을 사용할 수 있도록 도와준다.
- Spring framework의 경우 다양한 bean에 대한 설정이 필요하다(XML, JAVA 코드)
  - component scan(DTO, DAO, Service)
  - Dispatcher Servlet
  - View Resolver 등

- 의존 라이브러라나 프레임워크의 버전관리를 해준다.
  - Spring Boot는 pom.xml에 의존할 라이브러리나 프레임워크 종속성만 포함시키고 바로 진행할 수 있게한다.== 알아서 버전 관리를 해준다.



## DI(Dependency Injection)

| Java Bean                                                    | Spring Bean                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 데이터 표현을 목적으로 하는 자바 클래스                      | Spring Container에 의해 등록, 생성, 조회, 관계 설정이 되는 객체 |
| 생성자, getter/setter로 직렬화가 가능한 클래스               | IoC 방식으로 관리되는 객체                                   |
| 객체 생성->의존성 객체 생성(클래스 내부에서)-> 의존성 객체 메소드 호출 | 객체 생성 -> 의존성 객체 주입 -> 의존성 객체 메소드 호출     |
| 사용자가 제어                                                | 사용자가 아닌 spring이 제어                                  |



## Container

- 프로그래머가 작성한 코드의 처리과정을 위임받아 독립적으로 처리하는 존재

- 객체간의 의존성을 낮추기 위해 사용된다.

  #### 종류

- BeanFactory

  - Bean을 생성하고 관리하는 인터페이스 
  - 클라이언트의 요청이 있을 때 객체를 생성 -> Lazy init

- ApplicationContext
  - BeanFactory를 상속받은 인터페이스, 더 많이 사용된다.
  - 컨테이너가 구동되는 시점에 등록된 Bean객체들을 모두 스캔하여 생성 ->Eager init







참고

[](https://cofived.tistory.com/39)

 [](http://blog.naver.com/PostView.nhn?blogId=sthwin&logNo=221271008423&parentCategoryNo=&categoryNo=50&viewDate=&isShowPopularPosts=true&from=search)

[](https://sehun-kim.github.io/sehun/springbean-lifecycle/)