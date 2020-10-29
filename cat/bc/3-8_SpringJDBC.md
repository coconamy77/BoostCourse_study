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







참고 : [부스트코스](https://www.edwith.org/boostcourse-web/lecture/20660/)