---
title: 如何理解Java采用Unicode编码
date: 2020-05-26 20:15:51
tags:
- Java
categories:
- Java
mathjax: true
---

Java中字符仅以一种形式存在，那就是Unicode。由于Java采用unicode编码，char 在java中占2个字节。2个字节（16位）来表示一个字符。
这里的Java中是指**在JVM中、在内存中、在代码里声明的每一个char、String类型（String底层是char数组）的变量中**。



JVM的这种约定使得一个字符分为两部分：**JVM内部和OS的文件系统**。在JVM内部，统一使用Unicode表示，当这个字符被从JVM内部移到外部（即保存为文件系统中的一个文件中的内容时），就进行了**编码转换**，使用了具体的编码方案。因此可以说所有的编码转换只发生在边界的地方：**JVM和OS的交界处，也就是各种输入/输出流（或者Reader，Writer类）起作用的地方**。

所有的I/O基本上可以分为两大阵营：面向字符的输入/输出流和面向字节的输入/输出流。**如果面向字节**，那么这类工作要保证系统中的文件二进制内容和读入JVM内部的二进制内容一致，不能变换任何0和1的顺序。这种输入/输出方式很适合诸如视频文件或者音频文件，因为不需要变换任何文件内容。而面向字符的I/O是指希望系统中的文件的字符和读入内存的“字符”要一致。例如：我们的系统上有一个UTF-8的文本文件，其中有一个“永”字，我们不关心这个字的UTF-8编码是什么，只希望在使用面向字符的I/O把它读入内存并保存在一个char型变量中时，I/O系统不要直接把“永”字的UTF-8编码放到这个字符（char）型变量中，我们不关心这个char型变量具体的二进制内容到底是多少，只希望这个字符读进来之后仍然是“永”字。
从这个意义上可以看出，面向字符的I/O类，也就是Reader和Writer类，实际上隐式做了编码转换，在输出时，将内存中的Unicode字符使用平台默认编码方式（系统一般是GBK，IDE中可以设置）进行了编码，而在输入时，将文件系统中已经编码过的字符使用默认编码方案进行了还原。这里要注意，FileReader和FileWriter只会使用这个默认的编码来做转换，而不能为一个FileReader和FileWriter指定转换时使用的编码。这也意味着，如果使用中文版Windows系统，其中存放了一个UTF8编码的文件，当采用Reader类来读入的时候，它还会用GBK来转换，转换后的内容当然不对。这其实是一种傻瓜式的功能提供方式，对大多数初级用户（以及不需要跨平台的高级用户，windows一般采用GBK，linux一般采用UTF8）来说反而是一件好事。
如果用到GBK编码以外的文件，就必须采用编码转换：一个字符与字节之间的转换。因此，Java的I/O系统中能够指定转换编码的地方，也就是在字符与字节转换的地方，那就是InputStremReader与OutputStreamWriter。这两个类是字节流和字符流的适配器类，它们承担编码转换的任务。
既然java是用unicode来编码字符，"我"这个中文字符的unicode就是2个字节。


InputStream和Reader都可以用来读数据(从文件中读取数据或从Socket中读取数据)，最主要的区别如下: 

InputStream用来读取二进制数(字节流)，而 Reader用来读取文本数据，即 Unicode字符。那么二进制数与文本数据有什么区别呢?**从本质上来讲，所有读取的内容都是字节，要想把字节转换为文本，需要指定一个编码方法**。而 Reader就可以把字节流进行编码从而转换为文本。当然，这个转换过程就涉及编码方式的问题，它默认采用平台（系统或IDE）默认的编码方式对字节流进行编码，也可以显式地指定一个编码方式，例如“UTF-8″。



如何查看系统的默认编码格式：

```java
System.out.print(System.getProperty("file.encoding")); 
```

这里要注意：**如果在IDE里面编译运行的话，输出的默认编码是IDE里面设置的（比如IntelliJ IDEA里面可以设置project encoding，这就是默认的编码）；如果在DOS命令窗口里面编译运行的话，就是windows系统默认的编码，此时就是GBK！！！**



下面我们应该讨论一下OutputStream和Writer**在输出文件时采用什么编码**？

- OutputStream
  - **字节数组的编码方式，决定了OutStream写出文件的格式**。比如说从某个文件里利用InputStream流读取到byte数组中，如果该文件是UTF8编码，那么利用FileOutStream写出的文件的格式也就是UTF8；或者通过String.getBytes(指定字符集)获取字节数组，那么输出的文件的格式就是该指定的字符集
- Writer
  - **Writer 的编码是根据你的运行平台来获取编码**。如果你在dos窗口中运行java程序，则根据你的windows 编码决定（默认是GBK）；如果，你是在IDE开发工具中运行（可以自己设置），则跟着你的开发工具的编码走



**总而言之，一定要注意编码的问题，否则很容易出现乱码！！！**