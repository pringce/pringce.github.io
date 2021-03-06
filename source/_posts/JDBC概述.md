---
title: JDBC概述
date: 2020-06-09 10:04:17
tags:
- JDBC
categories:
- JDBC
mathjax: true
---

## JDBC是什么

- Java DataBase Connectivity（Java语言连接数据库）
- JDBC本质是SUN公司制定的**一套接口**（interface）
  - 在java.sql.*中（这个软件包下有很多接口）
- **任何一个接口都有调用者和实现者**，面向接口调用和面向接口写实现类，都属于面向接口编程
- 为什么SUN制定一套JDBC接口？
  - 因为每一个数据库的底层实现原理都不一样，Oracle、MySQL等DBMS的底层原理都不同。如果没有这套接口，那么在连接不同的DBMS时需要写不同的java代码，很繁琐。
  - 接口的实现类由各大DBMS厂商（Oracle、MySQL等）面向JDBC接口完成，而我们Java程序员（**调用者**，各大厂商是实现者）只需要面向JDBC接口调用即可，不用我们来写实现类
  - 各大厂商的JDBC接口实现类需要去对应的官网下载，这些实现类称之为**驱动**（MySQL驱动、Oracle驱动等）。
  - 所有的数据库驱动都是以jar包的形式存在，jar包当中有很多class文件，这些class文件就是对JDBC接口的实现。**驱动不是SUN公司提供的，是各大DBMS厂商负责提供**



## 配置驱动

先从官网下载对应版本的驱动！！然后将其配置到环境变量classpath当中（这个是针对采用文本编辑器开发的方式，使用IDEA工具的时候，不需要配置以上的环境变量。IDEA有自己的配置方式）

**如果使用IntelliJ IDEA，就不需要配置环境变量classpath了**，直接将其添加到project structure --> module --> 模块依赖包中！！如下图所示：

{% asset_img 1.png %}



## JDBC编程六步

- **注册驱动**：告诉JVM即将要连接的是哪个品牌的数据库
- **获取连接**：表示JVM的进程和数据库的进程之间的通道打开了，这属于进程之间的通信，使用完之后一定要关闭通道
- **获取数据库操作对象**：获取执行sql语句的对象
- **执行sql语句**：主要执行DQL和DML
- **处理查询结果集**：只有当第四步执行的是select语句的时候，才有这一步
- **释放资源**：使用完资源之后一定要关闭资源，Java和数据库属于进程间的通信，开启之后一定要关闭