---
title: JDBC工具类封装
date: 2020-06-16 16:46:09
tags:
- JDBC
categories:
- JDBC
mathjax: true
---

我们前面学习JDBC六步，发现如果每次对数据库操作都要敲这些代码，实在太繁琐。能不能将其包装成一个工具类，来简化代码呢？**答案是可以！**

下面我们来编写JDBC工具类：

```java
public class DBUtil {
    
    /**
     * 注册驱动的代码应该放在静态代码块中
     * 如果将其放在getConnection()方法中，当程序调用了两次该方法
     * 就会注册两次驱动，然后这没有必要，只需要注册一次即可
     * 所以将其放在静态代码块中
     */
    static{
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 工具类的构造方法通常都是私有的，但不是必须的
     * 因为工具类当中的方法都是静态的，不需要new对象，直接采用类名调用
     * 私有化构造方法可以防止别人new工具类对象
     */
    private DBUtil(){}

    /**
     * 获取数据库连接对象
     * @return 连接对象
     */
    public static Connection getConnection() throws SQLException {
        String url = "jdbc:mysql://localhost:3305/run?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true"; //数据库IP地址
        String user = "root"; //用户名
        String password = "123"; //密码
        return DriverManager.getConnection(url,user,password);
    }

    /**
     * 关闭资源
     * @param conn 连接对象
     * @param ps 数据库操作对象
     * @param rs 查询结果集
     */
    public static void close(Connection conn, Statement ps, ResultSet rs){
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(ps!=null){
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



下面我们利用上述的封装类完成一次**模糊查询**：

```java
public class JDBCTest {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        try {
            //获取连接
            conn = DBUtil.getConnection();
            //获取预编译的数据库操作对象
            String sql = "select ename from emp where ename like ?";
            ps = conn.prepareStatement(sql);
            //执行sql，获取第二个字母是A的ename
            ps.setString(1,"_A%");
            rs = ps.executeQuery();
            //处理查询结果集
            while(rs.next()){
                String ename = rs.getString("ename");
                System.out.println(ename);
            }

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //释放资源
            DBUtil.close(conn,ps,rs);
        }
    }
}
```

可以看到使用工具类后，代码非常简洁！！