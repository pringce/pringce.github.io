---
title: MySQL常见问题汇总
date: 2020-03-09 19:12:14
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

本文主要记录在学习和应用MySQL中碰到的各种问题和解决方法，以备查用。

## 1. 如何查看端口号？

一般有两种方法可查看数据库：

- 查看MySQL配置文件my.ini

- ```java
  mysql> show variables like 'port';
  +---------------+-------+
  | Variable_name | Value |
  +---------------+-------+
  | port          | 3305  |
  +---------------+-------+
  ```

  

## 2. 如何修改端口号？

- 停止mysql服务

```java
net stop mysql80
```

- 打开MySQL根目录下my.ini文件，修改文件中的port值，注意两个地方[client]和[mysqld]
  - **注意，如果要修改一定要同步修改为同一个值。服务器的端口号和客户端的端口号一定要保持一致，这样才能保证它两能顺利建立TCP/IP连接**
- 重启mysql服务

```
net start mysql80
```



## 3. 修改my.ini遇到权限问题

由于修改my.ini文件需要管理员权限。如果没有权限会提示没有权限打开该文件。有两种解决方法：

- 发现没有权限，如果右键有用管理员打开，直接打开然后修改报错即可。
- 如果右键没有用管理员打开，就window下用管理员打开记事本，然后用记事本打开my.ini，修改然后保存即可。



## 4. 解决net start mysql启动,提示发生系统错误 5 拒绝访问

在cmd下运行net  start mysql 不能启动mysql！提示发生系统错误 5；拒绝访问！切换到管理员模式就可以启动了。所以我们要以管理员身份来运行cmd程序来启动mysql。

**如果每天都要启动mysql服务，这样不很麻烦？**所以：

- 右键cmd找到它所在的位置：如下图：

{% asset_img 微信图片_20200309194035.png %}

- 右击选择属性，选择快捷方式，再选择高级，在选择以管理员身份运行，再单击确定即可！

{% asset_img 微信图片_20200309194148.png %}



## 5. 大小写问题

SQL语句不区分大小写，比如select和SELECT是一样的，毫无区别。**但是表中的数据是区分大小写的**，比如表emp中有个人的姓名是'SMITH'。下面SQL语句在Oracle中会报错：

```mysql
mysql> select * from emp where name = 'smith'; // 报错
mysql> select * from emp where name = 'SMITH'; // 正确
```

但是上面那种方法，用小写的'smith'在MySQL中也能查出来数据，因为MySQL相对于Oracle是比较语法松散的

