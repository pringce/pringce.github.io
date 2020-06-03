---
title: class文件反汇编
date: 2020-03-16 15:49:51
tags:
- Java
categories:
- Java
mathjax: true
---

jdk自带的很多命令都很有用，今天就来简单介绍下jdk的javap命令，javap是jdk自带的反汇编器，使用此命令，

可以将 Java文件编译后的class文件反汇编进而看到 Java编译器给我们生成的字节码，以便我们能更好的分析代码

的执行过程和运行流程。



**使用方法：**

- 先写好一个demo.java文件

- 在cmd中进入到该java文件的目录下，然后使用javac demo.java将其编译，这时你会在当前目录看到一个demo.class文件

- 之后再cmd中输入javap -c demo命令，将其字节码文件进行反汇编。

