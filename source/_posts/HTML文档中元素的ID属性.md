---
title: HTML文档中元素的ID属性
date: 2020-07-04 11:12:00
tags:
- HTML
categories:
- HTML
mathjax: true
---



在HTML文档中，任何元素（节点）都有**id属性**，比如*< html>*、*< head>*、*< body>*、*< form>*等。id属性是该节点的唯一标识，所以**在同一个HTML文档当中id值不能重复**

**注意**：表单提交数据的时候，只和name有关系，和id没关系



**id属性有什么用？**

- javascript语言，可以对HTML文档中的任意节点进行增删改操作
- 在进行增删改之前，需要先拿到这个节点，通过id来获取节点
- id的存在让我们获取节点更方便



**HTML文档是一棵树（DOM树，即Document树），树上有很多节点，每一个节点都有唯一的id**

javascript主要就是对这棵DOM树进行增删改