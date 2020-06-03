---
title: MySQL表的基本操作
date: 2019-11-13 10:46:34
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

​		创建表的时候首先需要描述清楚这个表是什么样的，它有哪些列，这些列都是用来存什么类型的数据等等。表中的一行叫做一条**记录**，一列就做一个**字段**。

#### 展示当前数据库的表

​		下面的语句用于展示当前数据库中有哪些表：

```mysql
mysql> SHOW TABLES;
```



#### 创建表

##### 1、基本语法

创建一个表时至少需要完成下列事情：

（1）**给表起个名**

（2）**给表定义一些列，并且给这些列都起个名**

（3）**每个列都需要定义一种数据类型**

（4）**如果有需要的话，可以给这些列定义一些列的属性，比如不许存储NULL，设置默认值等，具体列可以设置哪些属性可以在后面再详细说一下**

创建表的基本语法如下：

```mysql
CREATE TABLE 表名(
	列名1 数据类型 [列的属性],
	列名2 数据类型 [列的属性],
	...
	列名n 数据类型 [列的属性],
);
```

**注意事项**：

- 表名在数据库当中一般建议以：t_ 或者tbl_ 开始
- 在**CREATE TABLE**后写清楚我们要创建的表的名称
- 在小括号 () 中定义这个表各个列的信息，包括列的名称、列的数据类型，如果有需要的话也可以定义这个列的属性(列的属性用中括号 [] 包起来的意思是这部分是可选的)
- 列名、数据类型、列属性之间用空白字符分开就好，然后各个列的信息之间用逗号分隔开



下面我们建个表为例展示一下：

```mysql
CREATE TABLE first_table(
	first_column INT,
	second_column VARCHAR(100)
);
```

执行结果如下：

```mysql
mysql> CREATE TABLE first_table (
    ->     first_column INT,
    ->     second_column VARCHAR(100)
    -> );
Query OK, 0 rows affected (0.02 sec)
```



**如何查看已建立的表的列属性信息？**

```mysql
// 第一种方式,这种方式可以查看表的创建语句
mysql> show create table first_table;
// 结果如下
CREATE TABLE `first_table` (
  `first_column` int(11) DEFAULT NULL,
  `second_column` varchar(100) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4

// 第二种方式，这种方式可以查看已创建的表的结构，但看不到创建语句
mysql> desc first_table;
```



##### 2、为建表语句添加注释

​		我们可以在创建表时将该表的用处以注释的形式添加到语句中，只要在建表语句最后加上**COMMIT**语句即可：

```mysql
CREATE TABLE 表名 (
    各个列的信息 ...
) COMMENT '表的注释信息';
```

比如我们可以这样写first_table的建表语句：

```mysql
CREATE TABLE first_table (
    first_column INT,
    second_column VARCHAR(100)
) COMMENT '第一个表';
```

注释没必要太长，言简意赅即可，毕竟是给人看的，让人看明白是个啥意思就好了。为了我们自己的方便，也为了阅读你创建的人的方便，请遵守一下职业道德，写个注释吧～

##### 3、IF NOT EXISTS

​		和重复创建数据库一样，如果创建一个已经存在的表的话是会报错的，我们来试试重复创建一下first_table表：

```mysql
mysql> CREATE TABLE first_table (
    ->     first_column INT,
    ->     second_column VARCHAR(100)
    -> ) COMMENT '第一个表';
ERROR 1050 (42S01): Table 'first_table' already exists
```

如果想要避免这种错误发生，可以在创建表的时候使用这种形式：

```mysql
CREATE TABLE IF NOT EXISTS 表名(
    各个列的信息 ...
);
```

##### **4、简单的查询和插入语句**

**简单的查询语句**

如果我们想查看某个表里已经存储了哪些数据，可以用下边这个语句：

```mysql
SELECT * FROM 表名;
```

比如我们想看看前边创建的first_table表中有哪些数据，可以这么写：

```mysql
mysql> SELECT * FROM first_table;
Empty set (0.01 sec)
```

很遗憾，我们从来没有向表中插入过数据，所以查询结果显示的是Empty set，表示什么都没查出来～



**简单的插入语句**

MySQL插入数据的时候是以行为单位的，语法格式如下：

```mysql
INSERT INTO 表名(列1, 列2, ...) VALUES(列1的值，列2的值, ...);
```

也就是说我们可以在表名后边的括号中指定要插入数据的列，然后在VALUES后边的括号中**按指定的列顺序**填入对应的值（**只要前后对的上就行，不一定非要遵循建表时的列的顺序**）。注意：**如果我们在插入的时候把表名后面的列省略，那么values中的值的类型和顺序必须和建表时候一模一样。**

我们来为first_table表插入第一行数据：

```mysql
mysql> INSERT INTO first_table(first_column, second_column) VALUES(1, 'aaa');
Query OK, 1 row affected (0.00 sec)
```

这个语句的意思就是我们要向first_table表中插入一行数据，first_column列的值是1，second_colum列的值是'aaa'。看一下现在表中的数据：

```mysql
mysql> SELECT * FROM first_table;
+--------------+---------------+
| first_column | second_column |
+--------------+---------------+
|            1 | aaa           |
+--------------+---------------+
1 row in set (0.00 sec)
```

**我们也可以只指定部分的列**，没有显式指定的列的值将被设置为NULL（**因为默认是default NULL，如果我们在建表的时候可以自己default给定默认值，此时insert语句成功后没有显示指定的列的值将被设置为我们自己设置的默认值**），NULL的意思就是此列的值尚不确定。比如这样写：

```mysql
mysql> INSERT INTO first_table(first_column) VALUES(2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO first_table(second_column) VALUES('ccc');
Query OK, 1 row affected (0.00 sec)
```

这两条语句的意思就是：

- 第一条插入语句我们只指定了first_column列的值是2，而没有指定second_column的值，所以second_colum的值就是NULL。
- 第二条插入语句我们只指定了second_column的值是'ccc'，而没有指定first_column的值，所以first_column的值就是NULL。

执行完这两条语句后，再看一下现在表中的数据：

```mysql
mysql> SELECT * FROM first_table;
+--------------+---------------+
| first_column | second_column |
+--------------+---------------+
|            1 | aaa           |
|            2 | NULL          |
|         NULL | ccc           |
+--------------+---------------+
3 rows in set (0.00 sec)
```



**批量插入**

每插入一行数据写一条语句也不是不行，但是对人来说太烦了，而且每插入一行数据就向服务器提交一个请求远没有一次把所有插入的数据提交给服务器效率高，所以MySQL为我们提供了批量插入记录的语句：

```mysql
INSERT INTO 表名(列1,列2, ...) VAULES(列1的值，列2的值, ...), (列1的值，列2的值, ...), (列1的值，列2的值, ...), ...;
```

也就是在原来的单条插入语句后边多写几条记录的内容，用逗号分隔开就好了，举个例子：

```mysql
mysql> INSERT INTO first_table(first_column, second_column) VALUES(4, 'ddd'), (5, 'eee'), (6, 'fff');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM first_table;
+--------------+---------------+
| first_column | second_column |
+--------------+---------------+
|            1 | aaa           |
|            2 | NULL          |
|         NULL | ccc           |
|            4 | ddd           |
|            5 | eee           |
|            6 | fff           |
+--------------+---------------+
6 rows in set (0.01 sec)
```



**INSERT IGNORE**

对于一些是主键或者具有`UNIQUE`约束的列或者列组合来说，它们不允许重复值的出现，比如我们把`t1`的`m1`列添加一个`UNIQUE`约束：

```mysql
mysql> alter table t1 modify m1 int(11) unique;
Query OK, 0 rows affected (0.12 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

因为`m1`列有了`UNIQUE`约束，所以如果待插入记录的`t1`列值与已有的值重复的话就会报错，比如这样：

```mysql
mysql> insert into t1 values(1,'xixi');
ERROR 1062 (23000): Duplicate entry '1' for key 'm1'
```



所以，**对于那些是主键或者具有UNIQUE约束的列或者列组合来说，如果表中已存在的记录中没有与待插入记录在这些列或者列组合上重复的值，那么就把待插入记录插到表中，否则忽略此次插入操作。**如下所示：

```mysql
mysql> insert ignore into t1 values(1,'xixi');
Query OK, 0 rows affected, 1 warning (0.06 sec)
```

对于批量插入的情况，`INSERT IGNORE`同样适用，比如这样：

```mysql
mysql> insert ignore into t1 values(1,'xixi'),(9,'haha');
Query OK, 1 row affected, 1 warning (0.03 sec)
Records: 2  Duplicates: 1  Warnings: 1

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    9 | haha |
+------+------+
3 rows in set (0.00 sec)
```

这个批量插入的语句中我们想插入`(1, 'xixi')`和`(9, 'haha')`这两条记录，因为`m1`列值为`1`的记录已经在表中存在，所以这个记录会被忽略，而`(9, 'haha')`这条记录被插入成功



**insert on duplicate key update**

对于主键或者有唯一性约束的列或列组合来说，新插入的记录如果和表中已存在的记录重复的话，我们可以选择的策略不仅仅是忽略该条记录的插入，也可以选择更新这条重复的旧记录。比如我们想在`first_table`表中插入一条记录，内容是`(1, '哇哈哈')`，我们想要的效果是：对于那些是主键或者具有UNIQUE约束的列或者列组合来说，如果表中已存在的记录中没有与待插入记录在这些列或者列组合上重复的值，那么就把待插入记录插到表中，否则按照规定去更新那条重复的记录中某些列的值。设计`MySQL`的大叔给我们提供了`INSERT ... ON DUPLICATE KEY UPDATE ...`的语法来实现这个功能：

```mysql
mysql> insert into t1 values(1,'xixi') on duplicate key update n1 = 'jack';
Query OK, 2 rows affected (0.09 sec)

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | jack |
|    2 | b    |
|    9 | haha |
+------+------+
3 rows in set (0.00 sec)
```

这个语句的意思就是，对于要插入的数据`(1, 'xixi')`来说，如果`t1`表中已经存在`m1`的列值为`1`的记录（因为`m1`列具有`UNIQUE`约束），那么就把该记录的`n1`列更新为`'jack'`

对于那些是主键或者具有UNIQUE约束的列或者列组合来说，如果表中已存在的记录中有与待插入记录在这些列或者列组合上重复的值，我们可以使用`VALUES(列名)`的形式来引用待插入记录中对应列的值，比方说下边这个INSERT语句：

```mysql
mysql> insert into t1 values(1,'rose') on duplicate key update n1 = values(n1);
Query OK, 2 rows affected (0.10 sec)

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | rose |
|    2 | b    |
|    9 | haha |
+------+------+
3 rows in set (0.00 sec)
```

其中的`VALUES(n1)`就代表着待插入记录中`n1`的值，本例中就是`'rose'`。有人就疑惑了，为什么不直接写成update n1 = 'rose'？是的，没有任何问题，但是在批量插入大量记录的时候该咋办呢？此时`VALUES(n1)`就 派上了大用场：

```mysql
mysql> insert into t1 values(1,'可乐'),(2,'红牛'),(9,'雪碧') on duplicate key update n1 = values(n1);
Query OK, 6 rows affected (0.04 sec)
Records: 3  Duplicates: 3  Warnings: 0

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | 可乐 |
|    2 | 红牛 |
|    9 | 雪碧 |
+------+------+
3 rows in set (0.00 sec)
```



#### 删除表

```mysql
mysql> drop table if exists 表名;
```

这里的if exists可以不要



#### 表的复制

**将查询结果当做表创建出来**

语法：

```mysql
mysql> create table 表名 as select语句
```



```mysql
mysql> create table emp1 as select * from emp;
mysql> select * from emp1;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)
```

也可以只复制部分列

```mysql
mysql> create table emp2 as select ename,job from emp;
mysql> select * from emp2;
+--------+-----------+
| ename  | job       |
+--------+-----------+
| SMITH  | CLERK     |
| ALLEN  | SALESMAN  |
| WARD   | SALESMAN  |
| JONES  | MANAGER   |
| MARTIN | SALESMAN  |
| BLAKE  | MANAGER   |
| CLARK  | MANAGER   |
| SCOTT  | ANALYST   |
| KING   | PRESIDENT |
| TURNER | SALESMAN  |
| ADAMS  | CLERK     |
| JAMES  | CLERK     |
| FORD   | ANALYST   |
| MILLER | CLERK     |
+--------+-----------+
14 rows in set (0.00 sec)
```



**也可以将查询结果插入到一张表中**：(表结构要求：表的列数要和查询结果中的字段数相同)

```mysql
mysql> create table dept1 as select * from dept;
mysql> select * from dept1;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)
mysql> insert into dept1 select * from dept;
mysql> select * from dept1;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
8 rows in set (0.00 sec)
```

**如果只想插入部分字段怎么办？**

```mysql
mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | c    |
+------+------+
3 rows in set (0.00 sec)

mysql> select * from t2;
+------+------+
| m2   | n2   |
+------+------+
|    2 | b    |
|    3 | c    |
|    4 | d    |
|    2 | r    |
+------+------+
4 rows in set (0.00 sec)

mysql> insert into t1(m1) select m2 from t2;
mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | c    |
|    2 | NULL |
|    3 | NULL |
|    4 | NULL |
|    2 | NULL |
+------+------+
7 rows in set (0.00 sec)
```

注意和全部字段的区别，没有插入的字段按照默认值插入处理。



#### 修改数据

语法格式：

```mysql
mysql> update 表名 set 字段名1=值1,字段名2=值2... where 条件;
```

注意：**没有where条件，整张表数据将全部更新**

```mysql
mysql> update dept1 set loc='beijing' where deptno=10;
Query OK, 2 rows affected (0.02 sec)

mysql> select * from dept1;
+--------+------------+---------+
| DEPTNO | DNAME      | LOC     |
+--------+------------+---------+
|     10 | ACCOUNTING | beijing |
|     20 | RESEARCH   | DALLAS  |
|     30 | SALES      | CHICAGO |
|     40 | OPERATIONS | BOSTON  |
|     10 | ACCOUNTING | beijing |
|     20 | RESEARCH   | DALLAS  |
|     30 | SALES      | CHICAGO |
|     40 | OPERATIONS | BOSTON  |
+--------+------------+---------+
8 rows in set (0.00 sec)
```



我们也可以使用`LIMIT`子句来限制想要更新的记录数量，使用`ORDER BY`子句来指定符合条件的记录的更新顺序，比如：

```mysql
mysql> update t1 set n1 = 'abc' order by m1 desc limit 1;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | abc  |
+------+------+
3 rows in set (0.00 sec)
```

上述语句就是想删更新`m1`列值最大的那条记录



#### 删除数据

语法格式：

```mysql
mysql> delete from 表名 where 条件;
```

注意：**没有条件将全部删除**

删除编号为10的部门数据

```mysql
mysql> delete from dept1 where deptno=10;
Query OK, 2 rows affected (0.10 sec)

mysql> select * from dept1;
+--------+------------+---------+
| DEPTNO | DNAME      | LOC     |
+--------+------------+---------+
|     20 | RESEARCH   | DALLAS  |
|     30 | SALES      | CHICAGO |
|     40 | OPERATIONS | BOSTON  |
|     20 | RESEARCH   | DALLAS  |
|     30 | SALES      | CHICAGO |
|     40 | OPERATIONS | BOSTON  |
+--------+------------+---------+
6 rows in set (0.00 sec)
```



我们也可以使用`LIMIT`子句来限制想要删除掉的记录数量，使用`ORDER BY`子句来指定符合条件的记录的删除顺序，比方说这样：

```mysql
mysql> delete from t1 order by m1 desc limit 1;
Query OK, 1 row affected (0.09 sec)

mysql> select * from t1;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
+------+------+
2 rows in set (0.00 sec)
```



#### 重点：怎么删除大表？

如果数据量很大，delete语句删除动则就几个小时，效率很低。

为什么delete效率低呢？

- 因为它在删除的时候，没有删除存储空间，就等于是那些存储数据的格子还在，回滚的时候数据还能回来。所以用delete删除后是可以后悔的~

删除大表语法：

```mysql
mysql> truncate table 表名;
```

该语法表示，下面的那些存储数据的格子连同数据一起被截断删除，不可回滚，永久丢失。**但是表的定义还在，还可以往里面添加数据。**删除后发现该表还在，即show tables找得到该表，只不过表中的数据没有了



**还有一种删除表的方式：**

```mysql
mysql> drop table 表名;
```

删除内容和定义，释放空间。简单来说就是**把整个表去掉**.以后要新增数据是不可能的,除非新增一个表。删除后发现表没有了，即show tables找不到该表



#### 表结构修改

实际开发中，一旦设计好表结构就很少修改了，很少很少用到，可以直接用Navicat工具修改即可。如果非要写SQL语句，就简单说一下语法：

- **添加字段**

```mysql
mysql> alter table 表名 add 字段名 数据类型 [约束条件] [FIRST|AFTER 已存在的字段名]
```

`FIRST` 为可选参数，其作用是将新添加的字段设置为表的第一个字段；`AFTER` 为可选参数，其作用是将新添加的字段添加到指定的`已存在的字段名`的后面。

- **修改字段数据类型**

```mysql
mysql> alter table 表名 modify 字段名 数据类型 [约束条件];
```

其中，`表名`指要修改数据类型的字段所在表的名称，`字段名`指需要修改的字段，`数据类型`指修改后字段的新数据类型。**不写约束条件就默认删除了已有的约束条件，如果想添加新的约束条件可在后面直接写即可，不会影响已经存在的约束条件**

- **修改字段名称**

```mysql
mysql> alter table 表名 change 旧字段名 新字段名 新数据类型 [约束条件]; 
```

其中，`旧字段名`指修改前的字段名；`新字段名`指修改后的字段名；`新数据类型`指修改后的数据类型，如果不需要修改字段的数据类型，可以将新数据类型设置成与原来一样，但数据类型不能为空。

- **删除字段**

```mysql
mysql> alter table 表名 drop 字段名;
```

其中，`字段名`指需要从表中删除的字段的名称。

- **修改表名**

```mysql
mysql> alter table 旧表名 rename [to] 新表名
```

其中，`TO` 为可选参数，使用与否均不影响结果。