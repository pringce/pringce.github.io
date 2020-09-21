---
title: HTML引入CSS样式的三种方式
date: 2020-07-06 09:31:51
tags:
- CSS
categories:
- CSS
mathjax: true
---

## 内联定义

第一种方式，在标签内部使用style属性来设置元素的CSS样式。语法格式为：

```html
<标签 style="样式名:样式值;样式名:样式值;..."></标签>
```



## 样式块方式

第二种方式，在head标签中使用style块，这种方式被称为样式块方式，语法格式为：

```html
<head>
    <style type="text/css">
        选择器{
            样式名:样式值;
            样式名:样式值;
            ...
        }
        选择器{
            样式名:样式值;
            样式名:样式值;
            ...
        }
    </style>
</head>
```



**选择器：**

- id选择器：只针对某一个id的元素设置样式，格式为**#id{}**
- 标签选择器：针对html文件中所有该标签，设置所有该标签的样式，格式为**标签名{}**
- 类选择器：针对同一class名的元素，设置其样式，**格式为.类名{}**。但注意，需要在写html元素时设置其类名属性，如：

```html
<input type="text" class="myclass">
<select class="myclass">
    <option>a</option>
    <option>b</option>
</select>
```

此时，上面两个元素是同一个类，所以使用类选择器便可以实现两个不同的元素的统一样式设置：

```html
<style type="text/css">
    .myclass{
        font : 20px;
    }
</style>
```



## 链入外部样式表文件

将样式写到一个独立的xx.css文件当中（定义方式和上述的样式块方式一样），在需要的html文件中的head标签内直接引入css文件，样式就引入了，**这种方式最常用**，这种方式易维护，维护成本较低。语法格式如下：

```html
<link type="text/css" rel="stylesheet" href="css文件的路径" />
```

