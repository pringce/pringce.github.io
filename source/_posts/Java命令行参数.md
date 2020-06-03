---
title: Java命令行参数
date: 2020-03-12 15:39:53
tags:
- Java
categories:
- Java
mathjax: true
---

# 命令行参数定义

刚学习Java时，我们都会接触到下面这个简单的程序，我们可以main函数中定义了一个字符串数组参数，这就称为**命令行参数**，但是我们运行程序时从来没有给这个参数传值，那么我们怎么给这个参数传值呢？

```java
public class hello{
    public static void main(String[] args){
        System.out.println("hello world");
    }
}
```

**这个args数组的长度为0**。这个数组是留给用户的，用户可以在控制台上输入参数，这个参数自动会被转换为String[] args数组



# 如何传入命令行参数？

可以在Eclipse中想main函数传递命令行参数，也可以在dos窗口运行java程序时传入命令行参数。下面分别介绍

## 在Eclipse中想main函数传递命令行参数

- 新建Java程序，输入命令行参数

```java
public class helloworld{
    public static void main(String[] args){
        for(String arg:args)
        	System.out.println(arg);
    }
}
```

- 从窗口中直接设置传入的值，选择"运行"->"调试配置"。如图所示

{% asset_img 微信图片_20200312155810.png %}

- 选择java应用程序->自变量，填入命令行参数，并点击运行

{% asset_img 微信图片_20200312155917.png %}

- 运行结果

{% asset_img 微信图片_20200312160107.png %}

## 在dos窗口运行Java程序时传入命令行参数

- 在cmd中编译上述helloworld.java文件

{% asset_img 微信图片_20200312160527.png %}

- 编译结束后会产生.class字节码文件

{% asset_img 微信图片_20200312160630.png %}

- 在命令行中运行helloworld.class文件，并传入命令行参数，以下是运行结果

{% asset_img 微信图片_20200312162347.png %}

**注意：如果命令行参数含有空格，要加双引号**