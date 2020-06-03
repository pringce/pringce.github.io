---
title: MySQL概述
date: 2020-04-09 09:52:11
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

**sql、DB和DBMS分别是什么，它们之间的关系？**

- DB：DataBase，数据库，数据库实际上在硬盘上以文件的形式存在
- DBMS：DataBase Management System，数据库管理系统，常见的有MySQL、Oracle、SqlServer等
- SQL：结构化查询语言，是一门标准通用的语言，标准的sql适合于所有的数据库产品。SQL语句在执行的时候，实际上内部也会进行编译，然后再执行SQL语句，SQL语句的编译由DBMS完成
- **总结**：**DBMS负责执行sql语句，通过执行sql语句来操作DB当中的数据**



**SQL语句分类**

- DQL（数据查询语言）：查询语句，凡是select语句都是DQL
- DML（数据操作语言）：insert，delete、update，对表当中的数据进行增删改
- DDL（数据定义语言）：create，drop，alter，对表结构的增删改
- TCL（事务控制语言）：commit提交事务，rollback回滚事务
- DCL（数据控制语言）：grant授权，revoke撤销权限等



