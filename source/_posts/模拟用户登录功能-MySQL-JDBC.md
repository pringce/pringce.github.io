---
title: 模拟用户登录功能(MySQL+JDBC)
date: 2020-06-15 16:14:00
tags:
- JDBC
categories:
- JDBC
mathjax: true
---

- 需求
  - 模拟用户登录功能的实现
- 业务描述
  - 程序运行的时候，提供一个输入的入口，可以让用户输入用户名和密码；用户输入用户名和密码之后，提交信息，java程序收集到用户信息，java程序连接数据库验证用户名和密码是否合法，如果合法显示登陆成功



首先我们先建一个表tbl_user：

```mysql
create table tbl_user(
    id int auto_increment,
    loginName varchar(255),
    loginPwd varchar(255),
    primary key(id)
);
```

然后插入几个数据：

```java
mysql> insert into tbl_user(loginName,loginPwd) values('zhangsan','123'),('lisi','123');

mysql> select * from tbl_user;
+----+-----------+----------+
| id | loginName | loginPwd |
+----+-----------+----------+
|  1 | zhangsan  | 123      |
|  2 | lisi      | 123      |
+----+-----------+----------+
2 rows in set
```

至此，数据已经准备完毕了！



首先完成用户输入界面功能的代码：

```java
private static Map<String, String> initUI() {
    Scanner s = new Scanner(System.in);
    System.out.print("用户名：");
    String loginName = s.nextLine();
    System.out.print("密码：");
    String loginPwd = s.nextLine();

    Map<String,String> userLoginInfo = new HashMap<>();
    userLoginInfo.put("loginName",loginName);
    userLoginInfo.put("loginPwd",loginPwd);

    return userLoginInfo;
}
```



然后完成用户名和密码验证是否合法功能的代码：

```java
private static boolean login(Map<String, String> userLoginInfo) {
    //JDBC代码
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    try {
        //注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        //获取连接
        String url = "jdbc:mysql://localhost:3305/run?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true"; 
        String user = "root"; 
        String password = "123"; 
        conn = DriverManager.getConnection(url,user,password);
        //获取数据库操作对象
        stmt = conn.createStatement();
        //执行sql语句
        String loginName = userLoginInfo.get("loginName");
        String loginPwd = userLoginInfo.get("loginPwd");
        String sql = "select * from tbl_user where loginName = '"+loginName+"' and loginPwd = '"+loginPwd+"'";
        rs = stmt.executeQuery(sql);
        //处理查询结果集
        if(rs.next())
            return true;
    } catch (ClassNotFoundException | SQLException e) {
        e.printStackTrace();
    }finally {
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(stmt!=null){
            try {
                stmt.close();
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
    return false;
}
```



最后完成主程序：

```java
public static void main(String[] args) {
    //初始化一个界面
    Map<String,String> userLoginInfo = initUI();
    //验证用户名和密码
    boolean flag = login(userLoginInfo);
    System.out.println(flag ? "登陆成功" : "登录失败");
}
```

至此，该功能就开发完毕了！！



**但是**，在测试的时候会发现有一个问题，我们输入如下的用户名和密码：

```java
用户名：fasd
密码：fasd' or '1' = '1;
```

很明显，我们的tbl_user表中是没有该记录的，本应该显示登录失败。但我们惊奇地发现居然提示登录成功，**这是为什么呢？**



其实我们分析一下上述代码的sql语句：

```java
String sql = "select * from tbl_user where loginName = '"+loginName+"' and loginPwd = '"+loginPwd+"'";
```

我们将上述代码拼接后发现sql语句实际长这样：

```mysql
select * from tbl_user where loginName = 'fasd' and loginPwd = 'fasd' or '1'='1';
```

由于and的优先级高于or，所以where后面的条件始终为true。因此就会显示登陆成功了，这种就叫做**SQL注入！！**



**SQL注入的根本原因是什么？**

- 用户输入的信息中含有sql语句的关键字，并且这些关键字参与sql语句的编译过程，导致sql语句的原意被扭曲，进而达到sql注入！



**如何解决SQL注入问题呢？**听我娓娓道来！

只要用户提供的信息不参与sql语句的编译过程就可以解决SQL注入问题了！！

因此不再使用java.sql.Statement了，而要使用java.sql.PreparedStatement！！

java.sql.PreparedStatement继承了java.sql.Statement，java.sql.PreparedStatement是属于预编译的数据库操作对象。**它的原理是预先对sql语句的框架进行编译，然后再给sql语句传值**



下面我们重写上面的login功能模块：

```java
private static boolean login(Map<String, String> userLoginInfo) {
    //JDBC代码
    Connection conn = null;
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
        //注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        //获取连接
        String url = "jdbc:mysql://localhost:3305/run?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true"; //
        String user = "root";
        String password = "123";
        conn = DriverManager.getConnection(url,user,password);
        //获取数据库操作对象
        //这里的?是占位符
        //SQL语句的框子，一个?代表一个占位符，一个?将来接收一个值
        //注意：占位符不能使用单引号括起来
        String sql = "select * from tbl_user where loginName = ? and loginPwd = ?";
        //程序执行到此处，会发送sql语句框子给DBMS，然后DBMS进行sql语句的预先编译
        ps = conn.prepareStatement(sql);
        //执行sql语句
        String loginName = userLoginInfo.get("loginName");
        String loginPwd = userLoginInfo.get("loginPwd");
        //给占位符?传值
        //第一个参数代表占位符下标，从1开始，第二个参数是要传的值
        ps.setString(1,loginName);
        ps.setString(2,loginPwd);
        //这里不要再传sql了，否则会再编译一次
        rs = ps.executeQuery();
        //处理查询结果集
        if(rs.next())
            return true;
    } catch (ClassNotFoundException | SQLException e) {
        e.printStackTrace();
    }finally {
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
    return false;
}
```

这样就解决了**SQL注入**的问题。因此我们以后要使用**PreparedStatement**这个类，而不要使用Statement这个类来创建数据库操作对象了！





**Statement和PreparedStatement对比**：

- Statement有SQL注入问题，PreparedStatement解决了SQL注入
- Statement是编译一次执行一次，PreparedStatement是编译一次可执行n次，所以PreparedStatement效率更高一些
  - 这是因为如果在DBMS中前后两次输入的sql语句完全一样，那么只需要第一次编译即可，第二次无需编译。由于Statement每一次都要拼接sql语句，因此每次运行传给DBMS的语句都不同，所以每一次都要编译。而PreparedStatement只传递一个sql语句框子，所以每一次都一样，只需要编译一次即可
- PreparedStatement会在编译阶段做类型的安全检查（这里指的是在IDEA里面会有红色下滑线报错，而Statement却无法显示出来。因为setString()方法只能传递String字符串，传递别的当然会报错）



**PreparedStatement进行增删改**：

- 增（只写一下sql语句怎么写，其余都是类似的）

```mysql
mysql> String sql = "insert into dept(deptno,dname,loc) values(?,?,?)";
```

- 改

```mysql
mysql> String sql = "update dept set dname = ?, loc = ? where deptno = ?";
```

- 删

```mysql
mysql> String sql = "delete from dept where dname = ?";
```

