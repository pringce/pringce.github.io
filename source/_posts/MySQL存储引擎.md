---
title: MySQL存储引擎
date: 2020-04-13 15:07:25
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

### 什么是存储引擎

存储引擎这个名字只有在MySQL中存在，Oracle中有对应的机制，但是不叫存储引擎，Oracle中没有特殊的名字，就是“表的存储方式”

MySQL支持很多存储引擎，每一个存储引擎都对应了一种不同的存储方式。每一个存储引擎都有自己的优缺点，需要在合适的时机选择合适的存储引擎



### 存储引擎的使用

- 数据库中的各表均被（在创建表时）指定的存储引擎来处理。 
- 服务器可用的引擎依赖于以下因素： 
  - MySQL的版本 
  - 服务器在开发时如何被配置 
  - 启动选项

- 为了解当前服务器中有哪些存储引擎可用，可使用SHOW ENGINES语句：

```mysql
mysql> SHOW ENGINES\G
```

- 在创建表时，可使用ENGINE选项为CREATE TABLE语句显式指定存储引擎。

```mysql
CREATE TABLE TABLENAME (NO INT) ENGINE = MyISAM;
```

- 如果在创建表时没有显式指定存储引擎，则该表使用当前默认的存储引擎 ，一般是InnoDB存储引擎
- 默认的存储引擎可在my.ini配置文件中使用default-storage-engine选项指定。
- 现有表的存储引擎可使用ALTER TABLE语句来改变：

```mysql
ALTER TABLE 表名 ENGINE = INNODB;
```

- 为确定某表所使用的存储引擎，可以使用SHOW CREATE TABLE或SHOW TABLE STATUS语句：

```mysql
mysql> SHOW CREATE TABLE emp\G

mysql> SHOW TABLE STATUS LIKE 'emp' \G
*************************** 1. row ***************************
           Name: emp
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 14
 Avg_row_length: 1170
    Data_length: 16384
Max_data_length: 0
   Index_length: 0
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2020-04-10 11:35:00
    Update_time: 2020-04-10 11:35:00
     Check_time: NULL
      Collation: utf8mb4_0900_ai_ci
       Checksum: NULL
 Create_options:
        Comment:
1 row in set (0.00 sec)
```

- MySQL默认使用的存储引擎是InnoDB方式
- MySQL8.0版本支持的存储引擎有9个





### 常用的存储引擎

#### MyISAM

Engine: MyISAM
Support: YES
Comment: MyISAM storage engine
Transactions: NO
 XA: NO
 Savepoints: NO

**MyISAM这种存储引擎不支持事务，但是不是默认的存储引擎**

它管理的表具有以下特征：

- 使用三个文件表示每个表：
  - 格式文件 — 存储表结构的定义（mytable.frm） 
  - 数据文件 — 存储表行的数据（mytable.MYD） 
  - 索引文件 — 存储表上索引（mytable.MYI）

- 灵活的AUTO_INCREMENT字段处理
- 可被转换为压缩、只读表来节省空间 



**优点**：可被压缩，节省存储空间，并且可以转换成只读表，提高检索效率

**缺点**：不支持事务



#### InnoDB

- InnoDB存储引擎是MySQL的缺省引擎。 
- 它管理的表具有下列主要特征：
  - 每个InnoDB表结构的定义在数据库目录中以.frm格式文件表示
  - 数据库中的所有表的数据和索引都存储在同一个InnoDB表空间tablespace中（这是默认情况下的。当然也可以为每一个表自定义一个独有的表空间）
  - 提供一组用来记录事务性活动的日志文件
  - 用COMMIT(提交)、SAVEPOINT及ROLLBACK(回滚)支持事务处理，因为支持事务，所以很安全
  - 提供全ACID兼容
  - 在MySQL服务器崩溃后提供自动恢复
  - 多版本（MVCC）和行级锁定
  - 支持外键及引用的完整性，包括级联删除和更新 

**优点**：支持事务、行级锁、外键等，这种存储引擎数据的安全得到保障（**MyISAM不支持事务、不支持外键**）



#### MEMORY

- 使用MEMORY存储引擎的表，其**数据和索引存储在内存中**，且行的长度固定，这两个特点使得MEMORY存储引擎非常快。
- MEMORY存储引擎管理的表具有下列特征：
  - 在数据库目录内，每个表均以.frm格式的文件表示。
  - 表数据及索引被存储在内存中（断电就没）。
  - 表级锁机制。
  - 不能包含TEXT或BLOB字段。
  - MEMORY存储引擎以前被称为HEAP引擎。 



### 选择合适的存储引擎

- MyISAM表最适合于大量的数据读而少量数据更新的混合操作。MyISAM表的另一种适用情形是使用压缩的只读表。
- 如果查询中包含较多的数据更新操作，应使用InnoDB。其行级锁机制和多版本的支持为数据读取和更新的混合操作提供了良好的并发机制。
- 可使用MEMORY存储引擎来存储非永久需要的数据，或者是能够从基于磁盘的表中重新生成的数据。



