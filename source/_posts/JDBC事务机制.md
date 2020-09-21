---
title: JDBC事务机制
date: 2020-06-16 11:23:16
tags:
- JDBC
categories:
- JDBC
mathjax: true
---

## JDBC事务机制

- JDBC中的事务是**自动提交**的。只要执行任意一条DML语句，则自动提交一次，这就是JDBC默认的事务行为。但是在实际的业务当中，通常都是N条DML语句共同联合才能完成的，必须保证它们这些DML语句在同一个事务中同时成功或者同时失败



## JDBC修改自动提交机制

下面我们演示一下如何修改JDBC默认的事务自动提交机制：

以银行转账业务为例展示，首先新建一个银行账户表tbl_act：

```mysql
create table tbl_act(
    actno int,
    balance double(7,2)
);
```

然后插入相关的数据：

```mysql
mysql> insert into tbl_act values(111,20000),(222,0);
Query OK, 2 rows affected

mysql> select * from tbl_act;
+-------+---------+
| actno | balance |
+-------+---------+
|   111 |   20000 |
|   222 |       0 |
+-------+---------+
2 rows in set
```



**我们先来看一下JDBC中的默认自动提交机制**

```java
public class Act {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            String url = "jdbc:mysql://localhost:3305/run?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true";
            String user = "root"; 
            String password = "123"; 
            conn = DriverManager.getConnection(url,user,password);

            //下面两条update语句必须同时成功或同时失败
            String sql = "update tbl_act set balance = ? where actno = ?";
            ps = conn.prepareStatement(sql);
            ps.setDouble(1,10000);
            ps.setInt(2,111);
            int count = ps.executeUpdate();

            //这里肯定会出现空指针异常，然后进入catch语句
            //导致下面那条update语句无法被执行
            String s = null;
            s.toString();

            ps.setDouble(1,10000);
            ps.setInt(2,222);
            count += ps.executeUpdate();
            System.out.println(count==2 ? "转账成功" : "转账失败");
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
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
}
```

上面特意在两条update语句中添加了一个空指针异常，从而导致进入catch子句，而下面那条update语句无法被执行。但此时我们再查看表tbl_act中的数据发现第一条update命令已经提交并且修改磁盘中的数据了：

```mysql
mysql> select * from tbl_act;
+-------+---------+
| actno | balance |
+-------+---------+
|   111 |   10000 |
|   222 |       0 |
+-------+---------+
2 rows in set
```

钱少了1万！！！这是十分不安全的！！！

因此可以验证JDBC中的事务是自动提交的，执行一条DML语句，就提交一条！！！



**修改默认的自动提交机制为手动提交机制**

如何修改呢？

- 在第二步获取连接的时候，将自动提交机制设置为手动提交

```java
conn = DriverManager.getConnection(url,user,password);
conn.setAutoCommit(false);
```

- 在需要提交事务的地方添加提交语句

```java
conn.commit();
```

- 当语句异常的时候需要事务回滚，所以在catch子句中添加回滚语句

```java
if(conn!=null) {
    try {
        conn.rollback();
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
}
```



具体代码如下：

```java
public class Act {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            String url = "jdbc:mysql://localhost:3305/run?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&rewriteBatchedStatements=true"; 
            String user = "root"; 
            String password = "123";
            conn = DriverManager.getConnection(url,user,password);
            //开启事务
            conn.setAutoCommit(false);

            //下面两条update语句必须同时成功或同时失败
            String sql = "update tbl_act set balance = ? where actno = ?";
            ps = conn.prepareStatement(sql);
            ps.setDouble(1,10000);
            ps.setInt(2,111);
            int count = ps.executeUpdate();
            
            ps.setDouble(1,10000);
            ps.setInt(2,222);
            count += ps.executeUpdate();
            System.out.println(count==2 ? "转账成功" : "转账失败");
            //提交事务
            conn.commit();
        } catch (Exception e) {
            if(conn!=null) {
                try {
                    //回滚事务
                    conn.rollback();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            e.printStackTrace();
        }finally {
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
}
```

至此，就解决了默认的事务自动提交机制可能存在的问题！！