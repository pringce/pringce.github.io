---
title: MySQL密码错误如何修改？
date: 2019-09-28 19:44:16
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

​		前两天我在用MySQL的时候，出现了一个小问题：就是我在注册完windows服务后，然后用net start MySQL80登录到数据库服务器，却无法使得客户端连接到服务器，即在输入完mysql -uroot -p后，会报如下的错误：



{% asset_img  20190605145711972.png %}

(这种问题一般是由于密码错误引起的)

​		于是我就查了一个解决方法，特此记录一下：

1、首先打开一个cmd窗口A：

- 首先关闭MySQL服务器

```
net stop MySQL80
```

- 无密码启动MySQL服务

```
mysqld --console --skip-grant-tables --shared-memory
```

2、再开一个cmd窗口B：

- 无密码登录（密码处直接enter）

  ```mysql
  mysql -uroot -p
  //注意下面出现密码时，直接enter即可连接至MySQL
  ```

- 免密码登录设置空密码

  - ​	设置空密码

  ```
  UPDATE mysql.user SET authentication_string='' WHERE user='root' and host='localhost';
  ```

  - ​	刷新

  ```
  flush privileges;
  ```

- 设置加密的密码 

  - ​	以caching_sha2_password加密密码并设置

  ```
  ALTER user 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
  ```

  - ​	刷新

  ```
  flush privileges;
  ```


- 关闭上面打开的两个cmd窗口。然后再打开一个cmd窗口，启动mysql服务：net start mysql80。然后再正常的用刚才重新设置的密码登录即可。