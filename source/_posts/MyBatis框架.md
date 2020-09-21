---
title: MyBatis框架
date: 2020-06-17 16:07:13
tags:
- MyBatis
categories:
- MyBatis
mathjax: true
---

## 框架概述

**软件开发三层架构**:

- **界面层（视图层）**：和用户打交道，主要功能是接收用户的数据，显示请求的处理结果（jsp和html主要用于显示结果，servet主要用于收集用户数据）
- **业务逻辑层**：接受了界面层传递过来的数据，计算逻辑，调用数据库，获取数据（Spring）
- **数据访问层（持久层）**：访问数据库，执行数据的查询、修改、删除等等。（MyBatis）



三层架构的交互关系如下图所示：

{% asset_img 1.png %}

即：**用户使用界面层 --> 业务逻辑层 --> 数据访问层 --> 数据库（mysql）**



**三层对应的包：**

- 界面层：controller包（servlet）
- 业务逻辑层：service包（包含各种Service类）
- 数据访问层：dao包（包含各种Dao类）



**三层对应的处理框架：**

- 界面层：SpringMVC
- 业务逻辑层：Spring
- 数据访问层：MyBatis