---
title: JDK和JRE间的那些事儿
date: 2020-03-20 11:43:14
tags:
- Java
categories:
- Java
mathjax: true
---

首先了解下这两者的概念

- JRE ：英文名称（Java Runtime Environment），我们叫它：Java 运行时环境。它主要包含两个部分，jvm 的标准实现和 Java 的一些基本类库。它相对于 jvm 来说，多出来的是一部分的 Java 类库。

- JDK ：英文名称（Java Development Kit），Java 开发工具包。jdk 是整个 Java 开发的核心，它集成了 jre 和一些好用的小工具。例如：javac.exe等。


显然，这两者的关系是：包含关系。**JDK 包含了 JRE。**
{% asset_img 1.jpg %}



在**JDK1.8（包含以前）的版本**中，安装成功后，会存在两个JRE文件夹：**.../Java/jre**和**.../Java/jdk/jre**

**这两个JRE有什么联系么？**

答案是：**没有联系**。甚至准确的来说，它俩是一样的，无论是用哪一个都是可以的。只是很多人习惯将会单独安装另一个 jre，虽然单独安装的 jre 也并没有被使用，原因可能就是刚开始大家都不清楚 jdk 和 jre 之间的关系，所以就默认的都安装上了。

在 jdk 的 bin 目录下，基本上都是一些可执行文件，并且它们还不大。其实这些可执行文件只是外层的一层封装而已，这样的目的是避免输入的命令过长。例如 javac.exe 内部调用的其实是 JDK 中 lib 目录中的 tools.jar 中 com.sun.tools.javac.Main 类，也就是说这些工具只是入口而已。而实际上它们本身又都是由 Java 编写的，**所以在 jdk 目录下的 jre 既提供了这些工具的运行时环境，也提供了我们编写完成的 Java 程序的运行时环境。**

所以，很明显，jdk 是我们的开发工具包，它集成了 jre ，**因此我们在安装 jdk 的时候可以选择不再安装 jre 而直接使用 jdk 中的 jre 运行我们的 Java 程序**。（但是大部分人都默认将两个都装上了）。但是如果你的电脑不是用来开发 Java 程序的，而仅仅是用来部署和运行 Java 程序的，那么完全可以不用安装 jdk，只需要安装 jre 即可。

**从Java 11后Oracle不再单独发布JRE和Server JRE了，并统一JDK名称为：Oracle JDK 。**也就是说**从Java11开始不再区分JDK和JRE，下载的jdk本身就是jre**，默认没有jre目录。11之后不再区分具体的jre和jdk，你非要自己做的话，你用jlink把所有java开头的jmod都打进去就是你要的jre了。

[为什么JDK11后不带JRE?](https://www.zhihu.com/question/296351428/answer/500599249)



**jdk本身就是jre的超集**，包含了jre，同时也提供了一些开发者工具

**jre是jvm的超集**，包含了jvm同时也提供了rt.jar，也就是runtime.jar

现在这些都没有了，就只提供一个jdk下载，不再区分jvm，jre和jdk

你下载下来的jdk本身就是一个大的jre（java runtime）

你要的功能jdk里面都有，只是jdk顺便也提供了更多的东西



Java开始没有JRE下载，只有JDK下载。但JDK是包含JRE的，所以Java程序仍旧可以正常运行。



**Jlink可以从JDK中分离JRE，创建一个更小的JRE。步骤如下：**

- 管理员模式打开Cmd，运行到jdk目录
- 输入下面指令

```java
bin\jlink.exe --module-path jmods --add-modules java.desktop --output jre
```

- 即可在JDK目录下生成一个JRE文件夹



**Java1.8及以前，服务器上是否只安装 JRE 就可以了？**（从Java11开始不存在这个问题，因为没有单独的JRE下载，全都要下载JDK）

理论上是可以的，但是有前提条件。

**服务器上只安装 JRE 的前提**：

- 发布到服务器上时所有文件都是编译好的文件，包括 JSP 文件
- 后期不在服务器上直接修改（修改后需要编译，必须要有JDK）

如果部署的项目都是编译后重新部署，不在服务器上直接修改的话是可以只安装 JRE 的。



综合考虑，为避免以后这样那样的麻烦事发生，服务器上还是安装 JDK 吧！毕竟项目后期维护才是主要的事情。

**在服务器上安装 JDK 的好处：**

- 可以编译 java 文件，方便后期维护
- 保证 JSP 文件修改后稳定运行