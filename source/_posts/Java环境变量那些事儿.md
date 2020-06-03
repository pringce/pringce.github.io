---
title: Java环境变量那些事儿
date: 2020-03-20 17:22:21
tags:
- Java
categories:
- Java
mathjax: true
---

虽然现在Java版本已经更新到Java14，且从Java11开始，不提供JRE的下载，只提供JDK的下载。但Java1.8由于其稳定性，依然具有广泛的应用。（注意：Java1.8在安装时会有两套JRE，一套在Java目录下，一套在Java目录下的JDK目录中）。

JDK自带的jre称为**专用jre**，后面独立安装的jre称为**公共jre**
如果安装了JDK的话，其实是没必要再安装公共jre的，公共jre的作用是**自动向系统和浏览器注册Java运行环境**，以及提供了一些Java更新服务，所以没必要再去单独安装这个公共jre，只要正常安装JDK，指定好环境变量后，就OK了



**从JDK11开始，无论是开发机器还是部署机器都需要下载JDK，且配置环境变量。因为不再提供JRE的下载了。**





**JDK7与JDK8安装的区别**

JDK7在安装公共jre时会在**System32**中放置ava.exe，javaw.exe，javaws.exe。而JDK8不会在system32里放置java.exe，javaw.exe，javaws.exe，所以在只安装了JDK8未做任何设置的情况下，应该是无法执行java.exe命令的，那么JDK8就只安装了jdk1.8的文件夹和jre1.8么，也不是，在 **C:\Program Files (x86)\Common Files\Oracle\Java\javapath**路径下可以找到JDK8版本的java.exe，javaw.exe，javaws.exe，只不过由于这个目录并不在PATH变量下（好气呀，我的path里面有这个哎，所以我默认可以用java.exe嘿嘿，但是javac还是不行的），所以命令行中java命令无法找到这里，当你把这个目录添加到PATH之后，就可以找到了。



下面先聊一聊**Java环境变量配置（主要是Path和ClassPath）**：

- 高级系统设置-> 环境变量-> 系统变量
- 新建JAVA_HOME变量，变量值为下载的JDK路径
- 新建CLASSPATH变量（**非必须**），变量值如下（最前面的点不能省略，它代表当前路径）

```java
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```

- 在Path变量中，加入如下路径

```java
%JAVA_HOME%\bin
```

至此，环境变量配置完毕。可以在DOS界面使用javac和java了



**下面介绍一下CLASSPATH这个环境变量**

默认情况下，ClassLoader从当前路径下加载.class字节码文件。当然，也可以让ClassLoader去某个指定路径下加载字节码文件，这时需要配置环境变量CLASSPATH。

CLASSPATH环境变量属于java语言中的环境变量，不属于windows操作系统（Path环境变量属于操作系统）。CLASSPATH是给ClassLoader类加载器指路的。

当CLASSPATH环境变量没有配置的话，类加载器默认从当前路径下找字节码文件；当CLASSPATH环境变量配置为某个指定路径之后，类加载器只去指定的路径中加载字节码。

**综上，CLASSPATH环境变量不是必须要配置的。**





**安装JDK时，有两套JRE的相关问题（JDK1.8版本及以前才存在这个问题）**

记得在环境变量path中设置%JAVA_HOME%/bin路径M么？这应该是学习Java的第一步吧，不设置的话javac和java是用不了的。确实jdk/bin目录下包含了所有的命令。

可是有没有人想过我们用的java命令并不是 jdk/bin目录下的而是jre/bin（**公共JRE**）目录下的呢？不信可以做一个实验，大家可以把jdk/bin目录下的java.exe剪切到别的地方再运行 java程序，发现了什么？一切OK！



***但是，明明没有设置 jre/bin目录到环境变量中啊？***



试想一下如果java为了提供给大多数人使用，他们是不需要jdk做开发的，只需要jre能让java程序跑起来就可以了，那么每个客户还需要手动去设置环境变量多麻烦啊？

 所以安装公共jre的时候安装程序自动帮你把公共jre的java.exe添加到了系统变量中，验证的方法很简单，大家看到了系统环境变量的 path最前面有“%SystemRoot%\system32;%SystemRoot%;”这样的配置，那么再去Windows/system32下面去看看吧，发现了什么？有一个java.exe。

**如果强行能够把jdk/bin挪到system32变量前面，当然也可以迫使使用jdk/jre里面的java.exe，不过除非有必要，我不建议大家这么做。**



**两套jre，Java程序具体执行时最后使用哪个jre？这个机制是什么？**

**这个问题只存在于Java1.8版本及以前，从Java11开始后就不存在了，因为没有JRE可下载，只有JDK。**

系统存在多套jre时，那么由谁来决定使用哪一套jre呢？这个重担就落在java.exe的身上。

比如如果在命令行中输入java xxx的时候，java.exe的任务就是在我们电脑系统中众多的jre中找到合适的jre来执行xxx。**java.exe依据以下顺序来寻找jre：**

- 当前目录下是否是JRE目录下的bin，适用于JRE\bin目录下的java.exe
- 父目录下是否存在JRE目录，适用于JDK\bin目录下的java.exe（D:\Java\jdk1.8.0_40\bin中的java.exe执行时只会使用D:\Java\jdk1.8.0_40\jre的jre，就是出于这个原因）
- 查询注册表HKEY_LOCAL_MACHINE\Software\JavaSoft\JavaRuntime Environment\，适用于system32以及C:\ProgramData\Oracle\Java\javapath下的java.exe



所以java.exe的执行结果与我们电脑里哪一个java.exe（搜索一下就会发现我们电脑里面也不止一个java.exe）被执行有很大的关系。

另外，**java.exe在找到合适的jre以后，还有一个验证版本的程序，也就是java.exe与jre的版本一致才可以执行**。如果出现版本不一致的问题，一定要记得两件事情：

- 哪一个java.exe被执行；
- java.exe找到哪一套jre。

只要这两件事情确定了，我们就抓住了问题的来龙去脉，理解起来也就轻而易举了。

有一篇博客也介绍了类似问题：[Windows的JDK与JRE，java.exe在哪里是谁干了什么](https://www.jianshu.com/p/ebe7456e0aea)，用来参考



当存在多个java.exe时，如何知道哪个java.exe被执行？

```java
where java
```

在cmd中输入where java，如果系统环境变量path中存在，就会输出相应的路径。



**在windows上实现多个JDK的共存解决办法**

- 安装两个版本的JDK，比如JDK6和JDK7
- 环境变量如下设置：
  - JAVA7_HOME = JDK7的安装路径
  - JAVA6_HOME = JDK6的安装路径
  - JAVA_HOME = %JAVA6_HOME%（注意:如果你想切换jdk，就在此处设置切换即可）
- 添加%JAVA_HOME%\bin到环境变量path中



**可能存在的问题**：如果先安装JDK1.6，再安装JDK1.7。未修改JAVA_HOME（仍然为JAVA6_HOME），但调用java -version指令显示1.7的版本。其实原因很简单，上面已经说过了。在安装JDK1.7时（本机先安装jdk1.6再安装的jdk1.7），自动将java.exe、javaw.exe、javaws.exe三个可执行文件复制到了C:\Windows\System32目录，这个目录在WINDOWS环境变量中的优先级高于JAVA_HOME设置的环境变量优先级。  

解决方案：删除C:\Windows\System32目录下的java.exe即可

解决后，使用的即为各JDK版本的专用JRE，而非共用JRE。