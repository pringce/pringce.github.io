---
title: IntelliJ IDEA中各种文件夹的标记
date: 2020-06-05 21:26:29
tags:
- Java
categories:
- Java
mathjax: true
---

在刚使用idea开发时大家会碰到同样都是文件夹，但是颜色标志都有些不一样，不同的颜色标志代表着文件夹有不同的用途。如下图：

{% asset_img 1.jpg %}



下面介绍一下这些文件夹的区别与用途:

- **source root（source folder）**

**放在模块下面的一个目录**

存放java源代码的文件夹，当然也包括一些package文件夹，还可以包含其他文件。项目构建后，source folder里面的java文件自动编译成class文件放到相应的**"/out/production/模块名/"**文件夹中，其他文件也会拷贝一份到"/out/production/模块/"相应的目录下，这个**"/out/production/模块名/"**就是**当前classpath根目录**

例如：一个project有ja1、ja2和ja3这三个模块，在ja1下面有一个src文件夹，该文件夹的类型是source folder。该文件夹下都是java源代码，如果存在一个包com，包里有一个test.java。那么经过编译后，test.class文件的路径为project\out\production\ja1\test.class。

- **test source root（test source folder）**

**与source root平级**

这个类型的文件夹也用来放置源码，不过是测试的源码（比如单元测）。test source文件夹可以帮助你将测试代码和产品代码分离开。

该目录下的java源代码经过编译后生成的class文件会放在**"/out/test/模块名/"**文件夹中，其他文件也会拷贝一份到"/out/test/模块/"相应的目录下，这个**"/out/test/模块名/"**就是**当前classpath根目录**

**这里需要注意一点**：test source root下面的java源文件可以导入（import）并使用source folder下面的类；但是source root下面的java源文件不能导入（import）并使用test source folder下面的类。其实也很好理解，test source folder是用来测试的，当然可以使用源代码的类；但是该测试代码与业务无关，所以不能使用test source folder下面的类也没有什么影响

- **resource folder**

**放在模块下面，与source folder、test source folder平级**

该类文件夹用于存放你的应用中需要用到的资源文件（如：图片、xml或者properties配置文件等）。

 在构建过程中，resource文件夹中的内容（文件和文件夹）均会按照原文件的样子被复制到**"/out/production/模块名/"**文件夹。和source文件夹一样，你可以定制你的resource文件夹的结构。

 PS：默认情况下，工程编译后，resource中的文件和文件夹会被放置在源码编译后的相同的文件夹（即"/out/production/模块名/"）中

- **test resource folder**

用于存放测试源码中关联的资源文件。除此之外，和resource没有区别。

**注意**：默认情况下，在构建过程中，test resource文件夹中的内容（文件和文件夹）均会按照原文件的样子被复制到**"/out/test/模块名/"**文件夹。



**注意**：上述四种文件夹如果被标记为普通文件夹，那么在项目构建过程中，生成的class文件和其它文件将不会被添加到out文件夹中。

