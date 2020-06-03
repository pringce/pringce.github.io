---
title: 各种运算对于NULL值的处理
date: 2020-04-09 16:56:14
tags:
- MySQL
- 个人博客
categories:
- MySQL
mathjax: true
---

我们首先来看一下MySQL默认对于NULL值怎么处理：

- 只要有NULL参与的运算结果一定是NULL。比如A是表中的一个列，如果列中存在NULL值，那么该行的运算结果也为NULL值。注意：这里如果有函数参与，比如COS(A)、CONCAT(A,'OK')，只要A中有NULL值，最终该行的运算结果一定是NULL值

```mysql
mysql> select A*12 from package;
/* 如果A中的一些行有NULL值，那么该行最终的运算结果一定是NULL值*/ 
```

- 五个聚集函数对于NULL值的处理
  - count(*)比较特殊，它会包含NULL
  - count(列名)会忽略NULL值
  - count(NULL)的结果是0
  - avg、max、min、avg全部忽略NULL值
  - avg(NULL)、max(NULL)、min(NULL)、avg(NULL)运算结果均为NULL
- 分组函数group by对于NULL值的处理
  - 不会忽略它，会将其单独作为组中的一项
- distinct对于NULL值的处理
  - 同group by



**那么碰到NULL值该怎么处理呢？**

场景：表emp，每个人的工资由两部分组成：基本工资+津贴。每个人的月基本工资sal一定不是NULL，而有的人的月津贴com可能为NULL。现在我们需要计算每个人的年薪。先说一下错误的处理方式：

```mysql
mysql> select name,(sal+com)*12 as yearsal from emp;
```

上述SQL语句可能存在一个问题：**如果有人的津贴为NULL，那么最终计算出来的年薪就为NULL。这显然是不符合要求的。**



于是，引入**ifnull(expression,alt_value)**函数，该函数代表如果该表达式的结果为NULL，那么就用0代替它。

上面的问题可以这样解决

```mysql
mysql> select name,(sal+ifnull(com,0))*12 as yearsal from emp;
```

