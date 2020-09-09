# DAO DTO VO

## DAO: Data Access Object

- DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트, ConnectionPool 
- Database에 접근을 하기위한 로직과 비즈니스 로직을 분리하기 위해서 사용
- DB에 대한 접근을 DAO가 담당하도록 하여 데이터베이스 엑세스를 DAO에서만 하게 되면 다수의 원격호출을 통한 오버헤드를 VO나 DTO를 통해 줄일 수 있고 다수의 DB 호출문제를 해결할 수 있습니다.

<br />

## DTO(=VO): Data Transfer Object(Value Object)

- 계층간 데이터 교환을 위한 자바빈즈
- VO는 DTO와 동일한 개념이지만 read only 속성을 가짐
- 로직을 갖고 있지 않는 순수한 데이터 객체이며 속성과 그 속성에 접근하기 위한 getter, setter 메소드만 가진 클래스

