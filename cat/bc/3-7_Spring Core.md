# 웹 앱 개발 - Spring Core

## Spring Framework

- 엔터프라이즈급 어플리케이션을 구축할 수 있는 가벼운 솔루션이자, 원-스탑-숍(One-Stop-Shop)
  - 원스탑숍은 한꺼번에 해결하는 상점을 얘기하는 것
- 원하는 부분만 가져다 사용할 수 있도록 모듈화가 잘 되어 있다.
- IoC 컨테이너
- 선언적으로 트랜잭션을 관리할 수 있다.
- 완전한 기능을 갖춘 MVC Framework를 제공
- AOP 지원
- 스프링은 도메인 논리 코드와 쉽게 분리될 수 있는 구조



## Spring IoC

### 컨테이너(Container)

- 인스턴스의 생명주기를 관리, 추가적인 기능을 제공

### IOC(Inversion of Control, 제어의 역전)

- 프로그램 흐름의 제어를 개발자가 하는 것이 아니라 다른 프로그램이 흐름을 제어하는 것

- 개발자가 만든 어떤 클래스나 메소드를 다른 프로그램이 대신 실행해주는 것

### DI(Dependency Injection, 의존성 주입)

- 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것

``` java
//DI 적용 전
class 엔진{
    
}

class 자동차 {
    엔진 v5 = new 엔진();
}
```


```java
//DI 적용 후
@Component
class 엔진{
    
}

@Component
class 자동차{
    @Autowired
    엔진 v5;
}
```

- 컨테이너가 v5변수에 인스턴스를 할당해준다.

##### Spring에서 제공하는 IoC/DI 컨테이너 

- BeanFactory : IoC/DI에 대한 기본 기능을 가지고 있다.
- ApplicationContext : BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천됩니다. 트랜잭션처리, AOP등에 대한 처리를 할 수 있습니다. BeanPostProcessor, BeanFactoryPostProcessor 등을 자동으로 등록하고, 국제화 처리, 어플리케이션 이벤트 등을 처리할 수 습니다.

- BeanPostProcessor : 컨테이너의 기본로직을 오버라이딩하여 인스턴스화 와 의존성 처리 로직 등을 개발자가 원하는 대로 구현 할 수 있도록 합니다.
- BeanFactoryPostProcessor : 설정된 메타 데이터를 커스터마이징 할 수 있습니다.



## Bean

- Spring IoC(Inversion of Control) 컨테이너에 의해 인스턴스화, 관리, 생성된다.

- 일반적인 Java클래스를 Bean클래스라고 보통 말한다.
  - 기본생성자를 가지고 있다.
  - 필드는 private하게 선언한다.
  - getter, setter 메소드를 가진다.
  - getName(), getName() 메소드를 name 프로퍼티(property)라고 한다.

##### Spring Bean Scope

- Spring은 빈을 생성할 때 기본적으로 싱글톤(Singleton)객체로 생성한다.
  - JVM안에서 스프링이 bean마다 하나의 객체를 생성하는 것을 의미
  - 주입받은 bean은 동일한 객체라는 가정하에서 개발한다.
- Prototype 
  - 모든 요청에서 새로운 객체를 생성하는 것
  - 의존성 관계의 bean에 주입될 때 새로운 객체가 생성되어 주입된다.
  - 정상적인 방식으로 gc에 의해 bean이 제거된다.
- 싱글톤으로 적합한 객체
  - 상태가 없는 공유 객체: 상태를 가지고 있지 않은 객체는 동기화 비용이 없다.
  - 읽기용으로만 상태를 가진 공유 객체
  - 공유가 필요한 상태를 지닌 공유 객체
  - 쓰기가 가능한 상태를 지니면서도 사용빈도가 매우 높은 객체
    - 장시간에 걸쳐 매우 많은 객체가 생성
    - 해당 객체가 매우 작은 양의 쓰기 상태를 가짐
    - 객체 생성비용이 매우 큼
- 비싱글톤으로 적합한 객체
  - 쓰기가 가능한 상태를 지닌 색체 : 쓰기가 가능한 상태가 많아서 동기화 비용이 객체 생성 비용보다 큼
  - 상태가 노출되지 않은 객체: 일부 제한적인 경우, 내부 상태를 외부에 노출하지 않는 빈을 참조하여 다른 의좀 객체와는 독립적으로 작업을 수행하는 의존 객체가 있다면 싱글톤보다는 비싱글이 적합

##### XML을 이용한 설정

- applicationContext.xml 생성 -> xml 형식의 빈 생성 파일 설정

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml"); //초기화

UserBean userBean = (UserBean)ac.getBean("userBean");
userBean.setName("jung");
```

#####  Java Config를 이용한 설정

```java
@Configuration
public class ApplicationConfig {
	@Bean
	public Car car(Engine e) {
		Car c = new Car();
		c.setEngine(e);
		return c;
	}
	@Bean
	public Engine engine() {
		return new Engine();
	}
}
```

```java
public class Main {
	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		   
		Car car = (Car)ac.getBean("car");
        //Car car = (Car)ac.getBean(Car.class); 클래스를 넣어줘도 실행됨!
        //Car을 반환하는 bean을 찾아서 실행
		car.run();
	}
}
```

##### ApplicationContext는 파라미터를 받아들이지 않는 bean 생성 메서드를 먼저 다 실행 후에, 가지고 있는 객체를 파라미터로 받는 메서드를 실행해서 새로운 객체를 생성한다.

```java
@Configuration
@ComponentScan("kr.or.connect.diexam01")//패키지 명을 꼭 적어준다.-> 이 패키지 안에서 알아서 생성
public class ApplicationConfig2 {
}
```

```java
@Component
public class Car {
	@Autowired//이것때문에 setter가 필요 없다!
	private Engine v8;
	
	public Car() {
	}
}
```

```java
@Component
public class Engine {
	public Engine() {
	}
}
```

```java
public class Main {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig2.class);
		   
		Car car = ac.getBean(Car.class);
		car.run();
	}
}
```









참고:[부스트코스](https://www.edwith.org/boostcourse-web/lecture/20655/)



