# 웹 앱개발 - SpringJDBC

- JDBC프로그래밍에서 반복되는 동작들을 스프링 프레임워크가 처리해준다.

### JDBC Template

- org.springframework.jdbc.core에서 가장 중요한 클래스
- 리소스 생성, 해지를 처리해서 연결을 닫는 것을 잊어 발생하는 문제 등을 피할 수 있도록 한다.
- 스테이먼트(Statement)의 생성과 실행을 처리
- SQL 조회, 업데이트, 저장 프로시저 호출, ResultSet 반복호출 등을 실행
- JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환

##### JDBC Template select

- 열의 수 구하기

```java
int rowCount = this.jdbcTemplate.queryForInt("select count(*) from t_actor");
```

- 변수 바인딩 사용하기

```java
int countOfActorsNamedJoe = this.jdbcTemplate.queryForInt("select count(*) from t_actor where first_name = ?", "Joe"); 
```

- String값으로 결과 받기

```java
String lastName = this.jdbcTemplate.queryForObject("select last_name from t_actor where id = ?", new Object[]{1212L}, String.class); //마지막 매개변수가 리턴타입
```

- 한 건 조회하기

```java
Actor actor = this.jdbcTemplate.queryForObject(
  "select first_name, last_name from t_actor where id = ?", new Object[]{1212L}, 
    new RowMapper<Actor>() {
    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
      Actor actor = new Actor();
      actor.setFirstName(rs.getString("first_name"));
      actor.setLastName(rs.getString("last_name"));
      return actor;
    }
});
```

- 여러 건 조회하기

```java
List<Actor> actors = this.jdbcTemplate.query("select first_name, last_name from t_actor",
  new RowMapper<Actor>() {
    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
      Actor actor = new Actor();
      actor.setFirstName(rs.getString("first_name"));
      actor.setLastName(rs.getString("last_name"));
      return actor;
    }
});
```

- 위 두 코드 합치기

```java
public List<Actor> findAllActors() {
  return this.jdbcTemplate.query( "select first_name, last_name from t_actor", new ActorMapper());
}

private static final class ActorMapper implements RowMapper<Actor> {
  public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
    Actor actor = new Actor();
    actor.setFirstName(rs.getString("first_name"));
    actor.setLastName(rs.getString("last_name"));
    return actor;
  }
}
```

##### JDBC Template insert

```java
this.jdbcTemplate.update("insert into t_actor (first_name, last_name) values (?, ?)",  "Leonor", "Watling");
```

##### JDBC Template **update** 

```java
this.jdbcTemplate.update("update t_actor set = ? where id = ?",  "Banjo", 5276L);
```

**JdbcTemplate delete**

```java
this.jdbcTemplate.update("delete from actor where id = ?", Long.valueOf(actorId));
```



### 그외의 JDBC를 보완하기위해 나온 기술들

##### MyBatis

- 쿼리를 직접 조작할 수 있고, 좀 더 쉽게 시도할 수 있다.

##### JPA 

- 개발자의 데이터 종속을 줄여주어, 객체지향적인 개발을 할 수 있도록 도와주지만, 배우기 까다롭고, 정해진 코드만 사용해야된다는 단점이 있다.



### DTO(Data Transfer Object)

- 계층(컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계증)간 데이터 교환을 위한 자바빈즈
- 로직을 가지고 있지 않고, 순수한 데이터 객체
- 필드, getter, setter를 가지고 있다.



### DAO(Date Access Object)

- 데이터를 조회, 조작하는 기능을 전담하도록 만든 객체
- 보통 데이터베이스를 조작하는 기능을 전담하는 목적으로 만들어진다.



### ConnectionPool

- 비용이 많이 드는 DB연결에 대해 미리 커넥션을 여러개 맺어둔 것
- 커넥션이 필요하면 커넥션 풀에게 빌려서 사용한 후 반납한다.
- 커넥션을 제때 반납하지 않으면 사용할 커넥션이 없어서 프로그램이 늦어지거나 장애를 발생시킬 수 있다.



### DataSource

- DataSource는 커넥션 풀을 관리하는 목적으로 사용되는 객체
- 커넥션을 얻어오고 반납하는 등의 작업을 수행



### DB사용 순서

1. 스프링 컨테이너인 ApplicationContext는 설정파일로 ApplicationConfig라는 클래스를 읽어들인다.
2. ApplicationConfig에는 componentScanAnnotaition이 DAO 클래스를 찾도록 설정
3. 찾은 DAO클래스는 Spring컨테이너가 관리
4. Application Context는 DBConfig 클래스를 import하게 된다.
5. DBConfig 클래스에서는 데이터 소스와 트랜잭션 매니저 객체를 생성
6. DAO는 필드로 NamedParameterJdbcTemplate과 SimpleJdbcInsert를 가지게 된다.
7. 





참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/20660/)