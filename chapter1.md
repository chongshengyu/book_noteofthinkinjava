# File

^

```java
try {
	conn = JdbcUtil.getConnection();
	stmt = conn.createStatement();
	sql = "SELECT COUNT(*) AS TraCount FROM trajectory WHERE UserId='"+userId+"'";
	ResultSet rs = stmt.executeQuery(sql);
	while(rs.next()){
		traCount = rs.getInt("TraCount");
	}
} catch (Exception e) {
        e.printStackTrace();
}
try{
	
}
```







