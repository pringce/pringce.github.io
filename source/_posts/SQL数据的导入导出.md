---
title: SQL数据的导入导出
date: 2020-04-09 10:54:09
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

利用数据库开发时，可能会存在下面的小需求：

- 我们想将数据库备份一下，以免后面有小白兔删库跑路~
- 想将一个数据库里面的表导入到另一个数据库里面
- 下班想回家加班，就把单位的数据库备份一份带回去开发



凡是上面的需求，都需要将数据库导出成sql脚本文件，会生成一个*.sql文件。下面我们分别讲解一下如何导出和导入



### 导出数据

其实很简单的，就是一条语句就可以了，首先我们打开cmd，**不用进mysql指令界面**，直接在windows的dos命令窗口按照下列格式将导出语句敲进去，然后再输入密码即可了。但有时候我们想导出整个数据库，有时候又想导出数据库里面的一个表，这个时候就会有细小的差别，下面分别说一下：

- **导出整个数据库**

```java
mysqldump -u用户名 -p 数据库名>路径+导出的文件名
// 举个例子，这里run是待导出的数据库，存储在D盘下，导出的文件名是run.sql
mysqldump -uroot -p run>d:/run.sql
```

- **导出一个表**

```java
mysqldump -u用户名 -p 数据库名 表名>路径+导出的文件名
// 举个例子，这里run是待导出的数据库，t1是run中待导出的表名，存储在D盘下，导出的文件名是t1.sql
mysqldump -uroot -p run t1>d:/t1.sql
```

- **导出多个表**

```java
mysqldump -u用户名 -p 数据库名 表名1 表名2>路径+导出的文件名
// 举个例子，这里run是待导出的数据库，t1、t2是run中待导出的表名，存储在D盘下，导出的文件名是t1.sql
mysqldump -uroot -p run t1 t2>d:/t.sql
```

- **导出一个数据库结构**

意味着只导出数据库中所有表的结构，不导出数据

```java
mysqldump -u用户名 -p -d 数据库名>路径+导出的文件名
mysqldump -uroot -p -d run>d:/run.sql
// -d表示没有数据
```

- **只导出一个表结构**

意味着只导出一个表中的结构，不导出数据

```java
mysqldump -u用户名 -p -d 数据库名 表名>路径+导出的文件名
mysqldump -uroot -p -d run t1>d:/t1.sql
// -d表示没有数据
```



### 导入数据

- 进入mysql控制台

```mysql
mysql -uroot -p
```

- 使用CREATE  DATRABSE [数据库名字]创建一个数据库，然后使用use [数据库名]选择要使用的数据库

```mysql
mysql> create database copy;
mysql> use copy;
```

- 直接使用source [所在的路径/*.sql] 将SQL文件进行导入

```mysql
mysql> source d:/t1.sql;
```

