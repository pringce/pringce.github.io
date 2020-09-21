---
title: Java如何查找Class
date: 2020-06-10 17:31:50
tags:
- Java
categories:
- Java
mathjax: true
---

# Java 命令如何查找 Class

当我们执行`java helloworld`这样一条指令时，它会依次进行如下步骤：

- 启动Java虚拟机JVM
- 生成三个类加载器，分别是启动类加载器（BootstrapLoader）、扩展类加载器（ExtClassLoader）和应用类加载器（AppClassLoader）
- 随后根据这三个类加载器依次去对应的路径下搜索加载helloworld.class文件，**顺序分别是启动类加载器 > 扩展类加载器 > 应用类加载器**。如果一旦加载成功，就停止搜索
  - 启动类加载器的搜索路径为：由sun.boot.class.path（系统属性）所指定的
  - 扩展类加载器的搜索路径为：是由java.ext.dirs（系统属性）来决定的
  - 用户类加载器的搜索路径为：由java.class.path指定

这就是**双亲委派机制**！！！



上述三个类加载器的搜索路径如下所示：

**Bootstrap classes** 它们是构成 java 平台的类，比如位于`JDK/jre/lib/`中的 rt.jar 和 其他一些重要的 jar 文件

**Extension classes** 位于 `JDK/jre/lib/ext` 目录下的 jar 文件

**User classes** 由开发者和第三方定义的 class 文件。通过 `-classpath` 命令行选项或者 `CLASSPATH` 环境变量指定位置

用户一般只需指定 user classes 的位置，Bootstrap classes 和 Extension classes 会被自动处理。



将user classes path 存储在 `java.class.path` 系统属性中，该系统属性的值按以下顺序决定：

1. 默认值，当前文件夹下的所有 class 文件。
2. CLASSPATH 环境变量中指定的值，覆盖上一设置。
3. 命令行选项 `-classpath`，`-cp` 指定的值，覆盖上一设置。
4. 如果通过 `-jar` 指定的 jar 文件中包含有指定了 `Class­-Path` 的 manifest 文件，所有 user class 文件都必须来自于指定的归档文件中，覆盖上一设置

从上到下依次覆盖，如果指定了CLASSPATH，便会覆盖默认路径；如果在java命令行里指定了-cp参数，便会覆盖系统设置的CLASSPATH，以此类推！！



如果执行一个class文件`java test`，该文件内部使用了别的类（比如java.util.Date），那么在执行到该处的时候，会依次使用三个类加载器去加载java.util.Date，这里会首先被启动类加载器在rt.jar中找到，所以后面两个加载器其实此时便使用不到！！但是如果使用了自定义的类，那么前两个加载器都会找不到，此时便会被应用类加载器加载到！！！**这种加载类的顺序一定要掌握！！！**这种加载机制叫做**双亲委派机制**！！



# javac 命令如何查找 Classes

`javac` 会在以下两方面使用到 class 文件：

1. 用于支持自身的运行，比如`tools.jar`。
2. 用于解析源代码中的引用。（被引用的类可以是 class 文件的形式或者源代码文件的形式）



如果想要编译某个java源文件，需要cd到该文件所在的目录，然后执行`javac 源文件`（**注意这里和java命令的区别，执行java命令时是由三个类加载器依次去指定路径加载需要的class文件**）。但是如果待编译的java源文件中还使用了别的类，那么就需要去指定的路径查找该类了，指定的路径见下文！！



在编译下面的Test类时，需要先查找到TestInterface接口类，获取TestInterface接口信息

**Test类代码**

```java
package main.java;
import main.java.ITest;
public class Test implements TestInterface{
    public void say(){
        System.out.println("hello world!");
    }
    public static void main(){
        new Test().say();
    }
}
```

**TestInterface接口代码**

```java
package main.java;
public interface ITest{
    public void say();
} 
```

在查找TestInterface的过程中有如下几种情况：

- 如果只找到了TestInterface.java，则编译之，并使用编译生成的TestInterface.class文件
- 如果只找到了TestInterface.class，则直接使用之
- 如果既找到了TestInterface.java又找到了TestInterface.class，则javac会比较.java文件和.class文件的时间戳，如果.java文件版本更高，则编译.java文件并使用生成的.class文件。
  



那么问题来了，**编译器该怎么去找这些class文件或者java源文件呢？** **（！！！重中之重！！！）**

介绍这个内容之前，我们先讲一下java的import机制！

java中有两种包的导入机制，分别为：

- **单类型导入**（single-type-import），例如import java.io.File;
- **按需类型导入**（type-import-on-demand），例如import java.io.*;

单类型导入比较好理解，仅仅是导入一个public类或接口

按需类型导入(  import java.io.*;   )，有人误解为导入一个包下的所有类，其实不然，看名字就知道，他只会按需导入，也就是说它并非导入整个包，而仅仅导入当前类需要使用的类。

既然如此是不是就可以放心的使用按需类型导入呢？非也，非也。因为单类型导入和按需类型导入对类文件的定位算法是不一样的

我们先了解以下java编译器对类文件的定位方法：

- java编译器会从启动目录(bootstrap)，扩展目录(extension)和用户类路径下去定位需要导入的类，而这些目录进仅仅是给出了类的顶层目录。编译器的类文件定位方法大致可以理解为如下公式：**顶层路径名 \ 包名 \ 文件名.class = 绝对路径**



下面我们就来具体说一下java编译器在编译时查找类的具体流程：

比如存在这样一个Test.java源文件

```java
package com;
import java.io.*;
import java.util.*;
import java.auto.Single;

public class Test{
    public static void main(String[] args){
        Single s = new Single();
    }
}
```

该源文件中使用了Single类，编译器查找该类的具体流程如下：

- 如果在编译单元（比如这里的Test.java）的顶部没有包声明，Java编译器首选会从无名包（顶部没有package语句）中搜索一个类型。**找到就停止搜索，否则跳到下一步继续搜索**（因为这里的Test.java顶部有package，所以不需要经历这一步，只有无包编译单元才会经历这一步）
- 如果存在单类型导入的话，会经历这一步，没有单类型导入则直接跳到下一步。如上面的Test.java，编译器将在三个部分中试图查找java.auto.Single：**启动目录(bootstrap)，扩展目录(extension)和用户类路径**。且先查找用户类路径，再查找启动目录和扩展目录。同样的，如果找到就停止搜索，否则继续跳到下一步搜索
- 如果到达这一步，在Test.java的·当前目录下（同包下）查找Single，对于这个例子，由于Test.java在com包下，需要在用户类路径中搜索com.Single。果找到就停止搜索，否则继续跳到下一步搜索。**注意**：如果有两个模块module1（Test.java在module1中）和module2，且它们存在依赖关系（等同于都添加到了CLASSPATH），那么不仅会查找module1下的com.Single，也会查找module2下的com.Single，这一点一定要注意！！
- 如果到达这一步，编译器将在三个部分中试图查找java.lang.Single（因为自动导入了java.lang包，默认按需导入方式）：**启动目录(bootstrap)，扩展目录(extension)和用户类路径**。且先查找用户类路径，再查找启动目录和扩展目录。同样的，如果找到就停止搜索，否则继续跳到下一步搜索
- 如果到达这一步，编译器将在三个部分试图查找java.io.Single和java.util.Single。**需要注意的地方就是，编译器如果找到java.io.Single类之后并不会停止下一步的寻找，而要把所有的可能性都查找完以确定是否有类导入冲突**。如果在查找完成后，编译器发现了两个同名的类，那么就会报错。要删除你不用的那个类，然后再编译。如果没找到该类也会报错。当且仅当只找到一个该类时才会正常编译

