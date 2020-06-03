---
title: union关键字
date: 2020-04-13 09:06:19
tags:
- MySQL
- 个人博客
categories:
- MySQL
---

先说一下union的作用：**可以将查询结果集相加**



需求：**找出工作岗位是MANAGER和SALESMAN的员工**

- 方法一

```mysql
mysql> select ename,job from emp where job = 'manager' or job = 'salesman';
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
```

- 方法二

```mysql
mysql> select ename,job from emp where job in ('manager','salesman');
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
```

- 方法三：**使用union**

```mysql
mysql> select ename,job from emp where job = 'manager' union select ename,job from emp where job = 'salesman';
+--------+----------+
| ename  | job      |
+--------+----------+
| JONES  | MANAGER  |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| MARTIN | SALESMAN |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)
```

注意：**这里的查询结果的顺序是不一样的，因为我们是拼接而成的，所以顺序是排好序的。**





有人可能就要问了，既然以前学的where子句中的or、in等操作符可以解决，那么为什么还要用union呢？

- **因为存在or、in等操作符解决不了的问题。比如两张毫无相干的表结果想连接，or、in等就解决不了**

比如：

```mysql
mysql> select ename from emp union select dname from dept;
+------------+
| ename      |
+------------+
| SMITH      |
| ALLEN      |
| WARD       |
| JONES      |
| MARTIN     |
| BLAKE      |
| CLARK      |
| SCOTT      |
| KING       |
| TURNER     |
| ADAMS      |
| JAMES      |
| FORD       |
| MILLER     |
| ACCOUNTING |
| RESEARCH   |
| SALES      |
| OPERATIONS |
+------------+
18 rows in set (0.00 sec)
```

这里emp和dept是两张毫无关系的表，如果想拼接在一块，必须使用unino。**注意：最终查询结果显示的列名是第一个select语句中查询的列名。如果想显示别的名字，只需要给第一个select语句中的查询列重命名即可。**

还要注意一点：

- **union两边的seelct语句中的查询的字段的数目必须一致**。比如union前面的select查询了两个字段，union后面的select查询的字段数目必须也是两个字段
- **建议：用union连接起来的各个查询语句的查询列表中位置相同的表达式的类型应该是相同的**。当然这不是硬性要求，如果不匹配的话，`MySQL`将会自动的进行类型转换，比方说下边这个组合查询语句：
- **使用`UNION`来合并多个查询的记录会默认过滤掉重复的记录。**举个例子

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

mysql> select m1,n2 from t1 union select m2,n2 from t2;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | c    |
|    4 | d    |
|    2 | r    |
+------+------+
5 rows in set (0.00 sec)
```

由于`t1`表和`t2`表都有`(2, b)、(3, c)`这两条记录，所以合并后的结果集就把他俩去重了。如果我们想要保留重复记录，可以使用`UNION ALL`来连接多个查询：

```mysql
mysql> select m1,n1 from t1 union all select m2,n2 from t2;
+------+------+
| m1   | n1   |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | c    |
|    2 | b    |
|    3 | c    |
|    4 | d    |
|    2 | r    |
+------+------+
7 rows in set (0.00 sec)
```



**有union时如何使用order by和limit呢？**

`组合查询`会把各个查询的结果汇总到一块，如果我们想对最终的结果集进行排序或者只保留几行的话，可以在组合查询的语句末尾加上`ORDER BY`和`LIMIT`子句，就像这样：

```mysql
mysql> select m1,n1 from t1 union select m2,n2 from t2 order by m1 desc limit 2;
+------+------+
| m1   | n1   |
+------+------+
|    4 | d    |
|    3 | c    |
+------+------+
2 rows in set (0.00 sec)
```

- **由于最后的结果集展示的列名是第一个查询中给定的列名，所以`ORDER BY`子句中指定的排序列也必须是第一个查询中给定的列名（别名也可以）**
- 组合查询并不保证最后汇总起来的大结果集中的顺序是按照各个小查询的结果集中的顺序排序的，也就是说我们在各个小查询中加入`ORDER BY`子句的作用和没加一样
- 不过如果我们只是单纯的想从各个小的查询中获取有限条排序好的记录加入最终的汇总，那是可以滴，比如这样：

```mysql
mysql> (SELECT m1, n1 FROM t1 ORDER BY m1 DESC LIMIT 1) UNION (SELECT m2, n2 FROM t2 ORDER BY m2 DESC LIMIT 1);
+------+------+
| m1   | n1   |
+------+------+
|    3 | c    |
|    4 | d    |
+------+------+
2 rows in set (0.00 sec)
```

