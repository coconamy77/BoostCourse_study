# 웹 프로그래밍 - JDBC

## JDBC(Java Database Connectivity)란?

### JDBC

- Java언어와 Database를 연결해준다.
- 자바를 이용한 데이터베이스 접속과 SQL 문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약

- 자바 프로그램 내에서 SQL문을 실행하기 위한 자바 API
- SQL과 프로그래밍 언어의 통합 접근 중 한 형태

- JAVA는 표준 인터페이스인 JDBC API를 제공

- Maven에 다음과 같은 의존성을 추가한다.

```xml
<dependency>   
  <groupId>mysql</groupId>   
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.45</version>
 </dependency>
```



### JDBC를 이용한 프로그래밍 방법

1. import java.sql.*;
2. 드라이버를 로드 한다.(사용할 DB 드라이버를 로드한다.)
3. Connection 객체를 생성한다.(연결, DB 접속)
4. Statement 객체를 생성 및 질의 수행(쿼리문 실행)
5. SQL문에 결과물이 있다면 ResultSet 객체를 생성한다.
6. 모든 객체를 닫는다.

```java
import java.sql.*; //1

public static Connection getConnection() throws Exception{
	String url = "jdbc:oracle:thin:@117.16.46.111:1521:xe"; //3-1
	String user = "smu";
	String password = "smu";
	Connection conn = null;
	Class.forName("oracle.jdbc.driver.OracleDriver"); //2
	conn = DriverManager.getConnection(url, user, password);//3-2
	return conn;
}

public List<GuestBookVO> getGuestBookList(){
    List<GuestBookVO> list = new ArrayList<>();
    GuestBookVO vo = null;
    Connection conn = null;
    PreparedStatement ps = null; //4-1
    ResultSet rs = null; //5-1
    try{
        conn = DBUtil.getConnection();
        String sql = "select * from guestbook";
        ps = conn.prepareStatement(sql); //4-2
        rs = ps.executeQuery(); //5-2, Select
        while(rs.next()){ //6
            vo = new GuestBookVO();
            vo.setNo(rs.getInt(1));
            vo.setId(rs.getString(2));
            vo.setTitle(rs.getString(3));
            vo.setConetnt(rs.getString(4));
            vo.setRegDate(rs.getString(5));
            list.add(vo);
        }
    }catch(Exception e){
        e.printStackTrace();
    }finally {
        DBUtil.close(conn, ps, rs); //7
    }		
    return list;		
}

public int addGuestBook(GuestBookVO vo){
    int result = 0;
    Connection conn = null;
    PreparedStatement ps = null;
    try{
        conn = DBUtil.getConnection();
        String sql = "insert into guestbook values("
            + "guestbook_seq.nextval,?,?,?,sysdate)";
        ps = conn.prepareStatement(sql);
        ps.setString(1, vo.getId());
        ps.setString(2, vo.getTitle());
        ps.setString(3, vo.getConetnt());
        result = ps.executeUpdate();
    }catch(Exception e){
        e.printStackTrace();
    }finally {
        DBUtil.close(conn, ps);
    }
    return result;
}

//7
public static void close(Connection conn, PreparedStatement ps){
		if (ps != null) {
			try {
				ps.close();
			} catch (SQLException e) {e.printStackTrace(); }
		}
		if (conn != null) {
			try {
				conn.close();
			} catch (SQLException e) {e.printStackTrace();}
		}
	}
```





### JDBC 클래스의 생성 단계

(DriverManager -> Connection -> Statement -> ResultSet)

1. DricerManager를 이용해서 Connection인스턴스를 얻는다.
2. Connection을 통해서 Statement를 얻는다.
3. Statement를 이용해 ResultSet을 얻는다.

- 닫을 때는 반대로!!



참고: [부스트캠프](https://www.edwith.org/boostcourse-web/lecture/16734/)