---
title: SQL查询语句的定义顺序和执行顺序
date: 2020-03-31 17:08:47
tags:
- MySQL
- 个人博客
categories:
- MySQL
---

我们前面介绍了查询语句的各个子句，但是除了`SELECT`之外，其他的子句全都是可以省略的。如果在一个查询语句中出现了多个子句，那么它们之间的顺序是不能乱放的，顺序如下所示：

```mysql
SELECT [DISTINCT] 查询列表
[FROM 表名]
[JOIN 表名]
[ON 连接条件]
[WHERE 布尔表达式]
[GROUP BY 分组列表 ]
[HAVING 分组过滤条件]
[ORDER BY 排序列表]
[LIMIT 开始行, 限制条数]
```

其中中括号`[]`中的内容表示可以省略，我们在书写查询语句的时候各个子句必须严格遵守这个顺序，不然会报错的！



**注意**：定义顺序和语句的具体执行顺序是**不一致的**。上述查询语句中各子句的执行顺序如下：

- from
- join
- on
- where
- group by
- having
- select
- distinct
- order by
- limit

以上每个步骤都会产生一个**虚拟表**，**该虚拟表被用作下一个步骤的输入**。这些虚拟表对调用者(客户端应用程序或者外部查询)不可用，**只有最后一步生成的表才会给调用者**。如果没有在查询中指定某一个子句，将跳过相应的步骤。

　　逻辑查询处理阶段简介（**个人理解：除了from是一次性将表加载进来，其余所有的关键字都是对虚拟表一行一行的进行处理**）：

　　1、 FROM：对FROM子句中的前两个表执行笛卡尔积(交叉联接)，生成虚拟表VT1。

　　2、 ON：对VT1应用ON筛选器，只有那些使为真才被插入到TV2。

　　3、 OUTER (JOIN):如果指定了OUTER JOIN(相对于CROSS JOIN或INNER JOIN)，保留表中未找到匹配的行将作为外部行添加到VT2，生成TV3。**如果FROM子句包含两个以上的表，则对上一个联接生成的结果表和下一个表重复执行步骤1到步骤3，直到处理完所有的表位置**。（这里需要注意一下，**from... join...on可以认为是一组语句，它们执行可以看做是同时的**）

　　4、 WHERE：对TV3应用WHERE筛选器，只有使为true的行才插入TV4。

　　5、 GROUP BY：按GROUP BY子句中的列表对TV4中的行进行分组，生成TV5。

　　6、 HAVING：对VT6应用HAVING筛选器，只有使为true的组插入到VT7。

　　7、 SELECT：处理SELECT列表，产生VT8。

　　8、 DISTINCT：将重复的行从VT8中删除，产品VT9。

　　9、ORDER BY：将VT9中的行按ORDER BY子句中的列表顺序，生成一个游标(VC10)。

​		10、LIMIT：从VC10的开始处选择指定数量或比例的行，生成表TV11，并返回给调用者。



## 特别注意

**列别名的使用问题。**

我们在定义列别名的时候是在select中定义列别名。可能存在使用列别名情况的子句包括：where、group by、having和order by。

- 对于SQL Server而言
  - 由于where、group by和having的执行顺序在select前面，所以他们不能使用列别名。而对于order by而言，它在select后才执行，所以可以使用别名。
  - 总结：只有order by才能使用别名
- 对于MySQL而言
  - 我们只需要记住MySQL比较特殊，它可以在group by、having和order by中使用列别名，在where中不能使用列别名
  - mysql特殊是因为mysql中对查询做了加强