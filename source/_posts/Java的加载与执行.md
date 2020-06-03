---
title: Java的加载与执行
date: 2020-03-18 16:38:32
tags:
- Java
categories:
- Java
mathjax: true
---

Java程序的运行包含两个非常重要的阶段

- 编译阶段
- 运行阶段

{% asset_img 微信图片_20200318165157.png %}

## 编译阶段

编译阶段主要的任务是检查Java源程序是否符合Java语法，符合Java语法则能够生成正常的字节码文件（xxx.class）;不符合Java语法规则则无法生成字节码文件。

字节码文件中不是纯粹的二进制，这种文件无法在操作系统中直接执行。



### 编译阶段的过程：

- 程序员在硬盘的某个位置新建一个.java扩展名的文件，该文件被称为Java源文件，源文件当中编写的是Java源代码，必须符合Java语法规范
- Java程序员使用 JDK当中自带的javac.exe命令进行Java程序的编译
  - javac怎么用？在哪用？
    - 在DOS命令窗口使用
    - javac使用规则：javac 源文件.java
  - javac是一个java编译器工具/命令
- **一个java源文件可以编译生成多个.class文件（由文件中所定义的类的数量决定，class文件的名字即为类名）**
- 字节码文件/class文件是最终要执行的文件，所以class文件生成之后，java源文件删除并不会影响java程序的运行



## 运行阶段

JDK安装之后，除了自带一个javac.exe之外，还有另一个工具，叫做java.exe，它主要负责运行阶段

- java.exe在哪里？怎么用？
  - 在DOS窗口使用
  - 使用方法：java 类名
    - 例如：硬盘上有一个A.class，就是java A



### 运行阶段的过程：

- 打开DOS窗口，输入java A
- java.exe命令会启动JVM，JVM会启动类加载器ClassLoader
- ClassLoader会去硬盘搜索A.class文件，找到文件后将该字节码文件装载到JVM中
- JVM中的解释器将A.class字节码文件解释称二进制数据文件
- 然后操作系统执行二进制文件和底层硬件平台进行交互