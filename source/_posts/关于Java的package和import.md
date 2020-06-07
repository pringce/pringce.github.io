---
title: 关于Java的package和import
date: 2020-04-08 15:36:02
tags:
- Java
categories:
- Java
mathjax: true
---

关于java语言当中的包机制：

- 包又称为package，java引入package这种语法机制主要是为了方便程序的管理，**不同功能的类被分别放到不同的软件包当中**，查找比较方便，管理比较方法，易维护

- 怎么定义package？

  - 在java源程序的第一行上编写package语句
  - package只能编写一个语句
  - 语法结构
    - package 包名；

- 包名的命名规范：

  - 公司域名倒序+项目名+模块名+功能名。采用这种方式重名的几率较低，因为公司域名具有全球唯一性
  - 例如：
    - com.bjpowernode.user.service；
  - 包名要求全部小写，包名也是标识符必须遵守标识符命名规则
  - 一个包对应的是一个目录，比如上面那个com.bjpowernode.user.service对应的是4个目录（目录之间使用”.“隔开）

- 使用了package机制后，怎么编译？怎么运行？

  - 使用了package机制之后，类名将发生改变

  ```java
  package com.bjpowernode.javase.day11;
  
  public class test{
      public static void main(String[] args){
          System.out.println("test");
      }
  }
  ```

  - 上面这个类在加入包机制之后，完整的类名将变成：com.bjpowernode.user.service.test。

  - 编译还是在java源文件路径下运行：javac test.java

  - 但**运行会出现问题**，如果直接java test会导致找不到该类。此时可以手动创建四个目录，从上到下依次是com、bjpowernode、user和service，然后将编译的test.class放在service中。随后返回到包含com的目录下运行：java com.bjpowernode.user.service.test即可运行成功

  - 另一种方式：

    - javac -d 编译之后的存放路径 java源文件的路径，例：

    ```java
    javac -d . test.java
    // -d 带包编译
    // . 代表编译之后生成的东西放到当前目录下
    // 将当前目录下的test.java文件编译后放到当前目录下的com/bjpowernode/user/service目录中（自动创建，不用手动创建）
    // 运行跟上面的方式一样
    ```

- **使用IntelliJ IDEA完成包机制将会非常简单，直接建包即可**



下面举个例子：

```java
// test01.java
package com.bjpowernode.user.service;

public class test01{
    
}

// test02.java
package com.bjpowernode.user.service;
public class test02{
    public static void main(String[] args){
        // 以下两种方式均可
        // 第一种，完整包名
        com.bjpowernode.user.service.test01 t = new com.bjpowernode.user.service.test01;

        // 第二种
        // 可以省略包名,因为test01和test02在同一个包中
        test01 tt = new test01();
    }
}

// test03.java
package com.bjpowernode;
public class test03{
    public static void main(String[] args){
        // 这里的包名不能省略
        // 下面代码编译会报错：当省略包名后，会在当前包下找test01
        // 实际上编译器去找com.bjpowernode.test01了，这个类不存在
        test01 t = new test01();

        // 下面正确
        com.bjpowernode.user.service.test01 t = new com.bjpowernode.user.service.test01;
    }
}
```

什么时候包名可以省略？

- **在同一个包下的时候可以省略**



但是上面如果不在同一个包下，每次都需要加包名，这样太累了！于是就引入了**import语法**

```java
// test01.java
package com.bjpowernode.user.service;

public class test01{
    
}

// test02.java
package org.apache;

public class test02{
    public static void main(String[] args){
        // 不能省略，必须要写完整，太麻烦！
        com.bjpowernode.user.service.test01 t = new com.bjpowernode.user.service.test01;
    }
}

// test03.java
package org.apache

import com.bjpowernode.user.service.test01;

public class test03{
    public static void main(String[] args){
        // 这里由于导入了包，所以可以省略包名
        test01 t = new test01();
    }
}
```

- impot语句用来完成导入其它类，同一个包下的类不需要导入。不在同一个包需要手动导入
- import语法格式：
  - import 完整类名；(导入特定类)
  - import 包名.*；(导入包中全部的类)
- import需要编写在package语句下面，class语句上面（**一人之下，万人之上**）
- 但为什么String不用导入就可以直接使用呢？
  - String类属于java.lang包，java.lang.*不需要手动引入，系统自动引入
  - java.lang：language语言包，是java语言的核心类，不需要手动引入
  - 除java.lang.*，其余所有的包在使用的时候必须要手动引入
- 什么时候需要import？
  - **不是java.lang包下，并且不再同一个包下的时候，需要import进行导入**



这里需要注意几个问题：

- import只能导入同一个module下面的类，不能导入其它module下面的类，除非建立了依赖关系

- 如果有两个不同的包中存在同名类，在另一个包中的测试文件test.java中不能同时引用上述两个类，只能引用其中某一个，否则会报错

- 如果有两个包，一个包P1中有test.java和A.java文件，另一个包P2中有A.java文件

  - 如果在test.java中不写import P2.A，则这里如果使用A代表的是P1中的A
  - 如果在test.java中写了import P2.A，则这里如果使用A代表的是P2中的A
  - 如果两个A类都想使用，可以先import P2.A，然后A代表的是P2中的A，使用P1.A代表P1中的A，这样就可以区分开了

- **其它module 的类需要add dependency on other modules之后，才能import该模块下的各种类。如果存在多个模块，需要指定模块的依赖顺序（如果不同模块下存在相同的包和类，那么在import的时候，会根据依赖顺序引入第一个有该同名包下的同名类）**。如何指定依赖顺序呢？以IntelliJ IDEA为例：File --> project sturcture --> modules，然后就可以添加修改依赖顺序了，如下图：

  {% asset_img 1.png %}