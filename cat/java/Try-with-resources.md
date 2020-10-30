## Try-with-resources

- java 버전 7부터 제공되는 기능
- try문안에 변수를 넣어 close처리를 해준다.
- 이전에는 try-catch-finally를 사용해서 처리해줬다.

```
public int addTodo(TodoDto todo) {
    int inserCount = 0;

    Connection conn = null;
    PreparedStatement ps = null;

    try {
        Class.forName("com.mysql.jdbc.Driver");
        conn = DriverManager.getConnection(DBInfo.DB_URL, DBInfo.DB_USER, DBInfo.DB_PASSWORD);

        ps = conn.prepareStatement(sql);

        ps.setString(1, todo.getTitle());
        ps.setString(2, todo.getName());
        ps.setInt(3, todo.getSequence());

        inserCount = ps.executeUpdate();
    } catch (Exception e) {
    } finally {
        if (ps != null) {
            try {
            	ps.close();
            } catch (SQLException e) {
            }
        }
        if (conn != null) {
            try {
           	 	conn.close();
            } catch (SQLException e) {
            }
        }
    }
    return inserCount;
    }
```

- 모든객체에서 close()를 호출하는것이 아니라 AutoCloseable상속받은 객체만 해당된다.
  - Statement는 해당, PreparedStatement는 Statement를 상속받았다.