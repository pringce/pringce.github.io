---
title: Java入门（含Java基本介绍、数据类型、流程控制和数组操作）
date: 2020-02-06 14:33:02
tags:
- Java
categories:
- Java
mathjax: true
---

# 1. Java介绍

​		Java是目前全球Top 1的程序开发语言，是SUN公司James Gosling为手持设备开发的嵌入式编程语言，原名Oak，1995年改名为Java正式推出，有最大的开发社区；目前广泛应用于企业和互联网后端开发、Android开发和大数据开发。



## 1.1 Java的特点

1. 一种面向对象的跨平台编程语言，语法比C++简单
2. 以字节码的形式运行在虚拟机上
3. 自带功能齐全的类库
4. 有非常活跃的开源社区支持



## 1.2 Java的优缺点

优点：

- **简单**(语法比C++简单)、**健壮**(垃圾收集器让内存管理更容易)、**安全**(字节码运行在虚拟机上，无法操作硬件，因此安全)
- 跨平台，一次编写，到处运行
- 高度优化的虚拟机

缺点：

- 语法比较繁琐
- 无法直接操作硬件(不适用于底层操作系统的开发)
- GUI效果不佳(不适用于桌面应用程序的开发)



## 1.3 Java语言特性(开源、免费、纯面向对象、跨平台)

- 简单性

相对而言的，例如Java中不再支持多继承，C++是支持多继承的，多继承比较复杂。C++中右指针，Java中屏蔽了指针的概念。Java语言底层是C++实现的，不是C语言。

- 面向对象
- 可移植性（跨平台）

**一次编译，到处运行**

- 多线程
- 健壮性

和自动垃圾回收机制有关，自动垃圾回收机制简称GC机制。Java语言运行产生的垃圾是自动回收的，不需要程序员惯性

- 安全性



## 1.4 Java的版本

- Java SE: Standard Edition(标准版)
- Java EE: Enterprise Edition(企业版)
- Java ME: Micro Edition(移动版)

{% asset_img  微信图片_20200206145352.png %}



## 1.5 Java的规范

什么是规范？

​		比如USB就是一个规范，它规定了电源的正负极、信号线等等



​		Java的规范是Java Specification Request，简称**JSR**。有个组织Java Community Process(**JCP**)，它负责维护JSR规范。

{% asset_img 微信图片_20200206150256.png %}



## 1.6 Java平台

​		Java本身是一种面向对象的语言，最显著的特性有两个方面，一是所谓的“**一次编译，到处执行**”（Compile once, run anywhere），能够非常容易地获得跨平台能力；另外就是**垃圾收集**（GC, Garbage Collection），Java通过垃圾收集器（Garbage Collector）回收分配内存，大部分情况下，程序员不需要自己操心内存的分配和回收。

**Java平台实际上就是运行在各种操作系统上的JVM(虚拟机)**

{% asset_img 微信图片_20200206151238.png %}



### JDK和JRE的区别

- JRE（Java Runtime Environment）是Java运行时环境。它是运行编译后的Java程序所需的一切包，包括Java虚拟机JVM、Java类库（都是class文件，在lib目录下打包成了jar）、Java命令和其它基础设施。但是，它不能用于创建新程序。总而言之**是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的，而不是面向开发者的**。
- JDK（Java Development Kit）顾名思义是java开发工具包，是程序员使用java语言编写java程序所需的开发工具包，是提供给程序员使用的。**它拥有JRE所拥有的一切，但也有编译器（javac）和工具（如javadoc和jdb）**。它能够创建和编译程序。



举个例子：程序员在本地开发时需要安装JDK。但将开发好的程序部署到客户端时，只需要在客户端安装JRE即可，无需再安装JDK。



## 1.7 Java为什么可以跨平台运行？

**Windows操作系统内核和Linux操作系统内核肯定不同，它们执行指令的方式也不同**。因此Java程序不能直接和操作系统打交道，因为Java程序只有一份，操作系统执行原理却不同。

**Sun团队让Java程序运行在一台虚拟机上，简称JVM（可认为是Java程序和操作系统之间的介质，屏蔽操作系统的差异）。JVM再和底层的操作系统打交道。**

​		

​		1.首先开发好的java文件经过编译器Compiler的编译变为.class文件（**字节码文件，非二进制文件**），然而这个.class文件并不是真正的本地可以执行的指令 我们可以把这个.class文件称之为“中间码”

​		2.不同的计算机操作系统有着相应的JVM 比如win32位的、win64位的、linux系统的，.class文件经过**Interpreter**（**解释器**，也就是JVM）的解释（或者称之为翻译），变为真正的本地可执行指令（“00101001001…”）

**总结**：“一处编译，到处运行”是因为程序的**中间码.class文件是标准的，一致的**，在各个系统对应的JVM上都可以被识别解释然后运行，所以可以实现跨平台



## 1.8 Java安装

1. 上官网下载对应版本的JDK

   官网网址：https://www.oracle.com/technetwork/java/javase/downloads/index.html

2. 不更改安装路径，直接安装在C盘

3. 配置JDK环境变量，将C:\ProgramFiles\Java\jdk\bin添加到系统path变量中。打开cmd，输入“javac -version”，如果出现版本号，即代表配置完成



# 2. Java程序基础

## 2.1 Java程序基本结构

**标识符**：在java源程序中凡是程序员有权利自己命名的单词都是标识符。标识符可以标识：类名、方法名、变量名、接口名、常量名等，且严格区分大小写，不能用关键字

标识符必须是英文字母、数字、下划线和美元符号$的组合；且不能以数字开头；

**标识符命名规范**（驼峰）：

- 类名、接口名：首字母大写，后面每个单词首字母大写
- 变量名、方法名：首字母小写，后面每个单词首字母大写
- 常量名：全部大写



Java的基本程序结构如下：

```java
/**
 * 可用来自动创建文档注释
 */
public class Hello {
    public static void main(String[] args){ //程序入口
        System.out.pringln("Hello world");
        /* 多行注释
        注释内容
        注释结束
        */
    }
} //class定义结束
```

​		

​		Java有三种注释（注释是解释说明作用，增加可读性，不会被编译到字节码文件中）：

- 单行注释：以双斜线//开头，到当前行尾
- 多行注释：以/* ... */表示，中间所有内容都被视为注释
- javadoc注释：写在类和方法定义处，可用于自动创建文档。

```java 
/**
* javadoc注释
*
*/
```

**javadoc注释可以被javadoc.exe提取解析生成帮助文档**



## 2.2 变量

**什么是变量？**

变量本质上是内存中的一块空间，这块空间有**数据类型、名字和字面值（数据）**。变量是内存中存储数据的最基本的单元。



**数据类型的作用？**

不同的数据有不同的类型，不同的数据类型底层会分配不同大小的空间。数据类型是知道程序在运行阶段应该分配多大的内存空间



**有了变量的概念后，内存空间得到了重复使用**

```java 
// 下面两个100是不同的内存空间
System.out.println(100);
System.out.println(100);

// 引入变量，可访问同一块内存空间
int i = 100;
System.out.println(i);
System.out.println(i);
```



```java
public class Hello {
    public static void main(String[] args){
        int n = 100; //基本类型
        String s = "Hello,world"; //对象
    }
} 
```

- 变量可以持有某个基本类型的数值，或者指向某个对象
- 定义变量

```java
变量类型 变量名;
```

- 变量可被重新赋值
- **在同一个作用域中，变量名不能重名（但不同作用域下可以重名，比如局部变量和成员变量，且局部变量屏蔽成员变量）**，但可以重新赋值

- **Java用一对大括号作为语句块的范围，称为作用域**，作为在作用域里定义的一个变量，它只有在哪个作用域结束之前才可使用。
- **下面举个容易错的例子**

```java
// 报错
public static void main(String[] args){
    String name = "l";
    {
        String name = "x";
    }
}

// 没有报错
public static void main(String[] args){
    {
        String name = "l";
    }
    String name = "x";
}
```

为什么第一段代码报错了，而第二段代码没有报错？

​		**因为第二种离开作用域，变量所分配的内存空间将被JVM回收，所以语法不会有错误，而第一种写法name并没有离开{}作用域，所以会语法错误。**

- **变量必须被定义赋值才能访问**，两种方式赋值：
  - 变量名 = 字面值；
  - 变量类型 变量名 = 字面值; （初始化）

```java 
public class hello{
    public static void main(String[] args){
        int i;
        System.out.println(i); //报错，因为没有赋值
    }
}
```

- 变量的分类：
  - 根据变量声明的位置类分类
    - 局部变量：在方法体当中声明的变量叫局部变量
    - 成员变量：在方法体外（类体内）声明的变量叫成员变量
- **成员变量没有手动赋值，系统会默认赋值。而局部变量不会，如果没有赋值就访问，会报错**。八种数据类型默认值：
  - byte、short、int、long：**0**
  - float、double：**0.0**
  - boolean：**false**
  - char：**'\u0000'**
  - 引用数据类型：**null**

## 2.3 数据类型

**数据类型的作用**

程序当中有很多数据，每一个数据都是有相关类型的，不同数据类型的数据占用空间大小不同。数据类型的作用是指导JVM在运行程序时给该数据分配多大的内存空间

Java数据类型分两种：

- 基本数据类型（4大类8小种）
- 引用数据类型



## 2.4 基本数据类型

- 整数类型：long、int、short、byte
- 浮点类型：double、float
- 布尔类型：boolean
- 字符类型：char


计算机内存的最小存储单元是字节（byte），一个字节时8位二进制数：00000000 ~ 11111111（0 ~ 255）。内存单元从0开始编号，称为内存地址。

### 2.4.1 整形

- byte：8位，1字节
- short：16位，2字节
- int：32位，4字节 
- long：64位，8字节

**Java语言当中的"整数型字面值"默认是int型，要让这个字面值被当做是long型，需要在后面加L**

Java语言中的整数型字面值有三种表示方式

- **十进制**，是一种缺省 默认的方式

```java 
int a = 10;
```

- **八进制**，需要以0开始

```java
int b = 010; // 8
```

- **十六进制**，需要以0x开始

```java
int c = 0x10; // 16
```



```java
byte b = 127; //byte范围：-128 ~ 127
short s = 32767; //short范围：-32768 ~ 32767
int i = 2147483647;
int i2 = -2147483648;
int i3 = 2_000_000_000; //加下划线更容易识别
int i4 = 0xff0000; //16进制表示的16711680
int i5 = 0b1000000000; //2进制表示的512
// 同一个数的不同进制表示完全相同
long l = 9000000000000000000L; //由于java默认类型为int，因此需要在结尾加L
```



有个小问题，java默认整数都是int型。按理说int型是不能赋值给short、byte、char类型的，但是下面却可以编译通过。这是为什么？在java中10这个常量到底是怎么存放的呢？

```java
short a = 10;
byte b = 10;
char c = 10;
```

因为在Java中规定所有整数默认是int型。但是只要在byte，short、char它们的取值范围内赋值都是可以的，比如byte=127 就是可以的，但是你给byte=128  就不行了，因为就超出byte 127 的最大范围了。char=数字并不代表那是一个数字，char赋值整数代表的是它背后的字符，因为每一个字符给它硬性对应一个整数值



### 2.4.2 浮点类型

- float：32位，4字节
- double：64位，8字节

在java语言当中，所有浮点型字面值默认被当做double类型来处理。要想该字面值被当做float类型来处理，需在字面值后面添加F/f。

```java
float f1 = 3.14f; // Java浮点数默认为double类型，float类型需在结尾加f
float f2 = 3.14e38f; // 科学计数法表示的3.14*10^38
double d = 1.79e308;
double d1 = 4.9e-324; // 4.9*10^(-324)
```

double的精度太低（相对来说），不适合做财务软件。财务涉及到钱的问题，要求精度较高。所以SUN公司在基础SE类库中为程序员准备了精度更高的类型，只不过这种类型是一种引用数据类型，不属于基本数据类型，它是**java.math.BigDecimal（定点数）**

### 2.4.3 布尔类型

只有true和false两个值，通常是计算结果，占用1个字节

不可以0或非 0 的整数替代false和true，这点和C语言不同

```java
boolean flag = 1; // 编译错误，不兼容的类型
```

**boolean类型不可以转换为其它的数据类型**

### 2.4.4 字符类型

char：保存一个字符，用单引号表示，**占用2个字节**，取值范围是 0 ~ 65535。如果输入多个字符编译器将会报错。注意区分字符类型和字符串类型的区别。

```java
char c1 = 'A';
// 或 char c1 = '\u0041';
char c2 = '中';
```

short和char所表示的种类总数是一样的，只不过char可以表示更大的正整数，因为char没有负数



char类型表示的是现实世界中的文字，文字和计算机二进制之间默认情况下不存在转换关系。为了让计算机可以表示现实世界中的文字，我们需要人为干涉，需要人提前制定好文字和二进制之间的对照关系，这种对照关系被称为：**字符编码**。计算机最初**只支持英文**，最先出现的字符编码是**ASCII码**：

'a' --> 97

'A' --> 65

'0' --> 48

**ASCII码共定义了128个字符，因此用一个字节即可完全表示**



**编码和解码采用同一套编码集，不会出现乱码。否则会出现乱码**



随着计算机的发展，后来出现了一种编码方式，是国际化标准组织ISO制定的，这种编码方式支持西欧语言，**向上兼容ASCII码**，仍然不支持中文。这种编码方式是：**ISO-8859-1**，又被称为**latin-1**



随着计算机向亚洲发展，计算机开始支持中文、日文、韩文等，其中支持简体中文的编码方式：**GB2312、GBK、GB18030**（容量从小到大，即支持的中文字符数量从小到大）



支持繁体中文：大五码（big5）



后来出现了一种编码方式统一了全球所有的文字，容量较大，这种编码方式叫做**unicode编码**。unicode编码方式有多种具体的实现：UTF-8、UTF-16、UTF-32等



**Java语言源代码采用unicode编码方式，所以标识符可以写中文**



**Java中用两个字节表示char，在char中存储的是字符的Unicode码点，char只能表示 BMP子符（BMP的定义见另一篇文章：[彻底弄懂Unicode编码那些事儿](https://pringce.github.io/2020/03/22/彻底弄懂Unicode编码那些事儿/#more)）。**

这里我也疑惑很久Java是如何表示单个的补充码字符（即SMP中的字符），后来明白Java给出的方式就是 “表示不了” ，这样也很合理，65536个字符几乎已经涵盖世界主流语言用到的所有字符了，没必要为了极少出现的字符而构建一个复杂的char类型。

**Java中的char本质上是UTF-16编码**。而UTF-16实际上也是一个变长编码（2字节或4字节）。如果一个抽象的字符在UTF-16编码下占4字节，显然它是不能放到char中的。换言之，char中只能放UTF-16编码下只占2字节的那些字符，即BMP中的字符。



#### 转义字符

反斜杠 \ 在 Java语言中具有转义功能，转义字符出现在特殊字符前，会将特殊字符转义成普通字符

\n：换行符

\t：制表符（Table键，与空格不同）

\\\\：反斜杠字符（'\\'这种会报错，因为\有转义功能，会与后面的单引号配对成转义字符，因此前面的单引号没人与它配对）

\\'：单引号字符（'''会报错，原因类似上面，后面两个单引号会配对）

\\"：双引号字符。这里需要注意一点

\u####：代表后面的一串是一个字符的Unicode编码



```java
System.out.println("\"Helloworld\""); //输出:"Helloworld"
//System.out.println(""Helloworld""); //报错
// 因此在字符串中表示双引号需要加反斜杠

char c = '"'; // 代表双引号，无需加反斜杠，因为没人跟他配对，这里需要多注意
```



## 2.5 数据类型转换

关于基本数据类型之间的互相转换，**转换规则**如下：

- 8种基本数据类型当中除boolean类型外，剩下的7种类型之间都可以互相转换

- 小容量可直接向大容量转换，称为**自动类型转换**。容量从小到大排序：

  byte < short/char < int < long < float < double

  {% asset_img 20180308134257570.png %}

  注意：任何浮点类型不管占用多少字节，都比整数类型容量大。char和short容量相同。

```java
int a = 100;
long b = a;
long c = 10;
```

- 大容量不能直接转换为小容量，需要加强制类型转换符，称为**强制类型转换**。但是在运行阶段**可能会损失精度**，所以谨慎使用。**强转原理：将左边的二进制位砍掉**

```java
long a = 100L;
// int b = a; // 编译错误
int b = (int)a; //编译正确
```

- 当整数字面值没有超过byte、short、char的取值范围，可以直接赋值给byte、short和char类型的变量。**但整型变量不能直接赋值给short、byte和char变量，也需要强制类型转换符**

```java
// byte a = 1000; // 错误，因为1000超出了byte的范围
byte a = 20; // 正确，因为20没有超出byte范围
/*
int r = 20;
short t = r; // 编译错误
short t = (short)r; // 编译正确
*/
```

- byte、short和char类型混合运算的时候，各自先转换成int类型再做运算

```java 
byte i = 5;
short j = 10;
//short k = i + j; //编译错误，short和byte运算，首先会转换成int再运算，所以i+j运算结果为int，int赋值给short就会出错
short k = (short)(i+j); // 编译正确
```

- 多种数据类型混合运算时，先转换成容量最大的那种类型再做运算
- 当把任何基本类型的值和字符串值进行连接运算时(+)，基本类型的值将自动转化为字符串类型。 

## 2.6 常量

常量就是用**final**修饰的变量：

- 常量初始化后不可再次赋值
- 常量名通常全部大写
- 常量用来避免意外赋值
- 常量用来替代Magic Number：魔数导致代码可读性差，修改不方便的问题。

```java
// 常量的好处就是如果需要更改常量的值，只需在定义处修改
final double PI = 3.14;
double r = 4.0;
double area = PI*r*r;

//magic number:增加可读性，后续能看懂
final double tax_rate = 0.2;
double pay = 1-tax_rate;
```

## 2.7 基本类型和引用类型

Java提供了两种变量类型：基本类型和引用类型。

- 基本类型：byte、short、int、long、float、double、char、boolean
- 引用类型：String、类、接口类型、数组类型、枚举类型、注解类型

区别：

- 基本数据类型在被创建时，在栈上给其划分一块内存，将数值直接存储在栈上。
- 引用数据类型在被创建时，首先要在栈上给其引用（句柄）分配一块内存，而对象的具体信息都存储在堆内存上，然后由栈上面的引用指向堆中对象的地址。
- Java 将内存空间分为堆和栈。基本类型直接在栈中存储数值，而引用类型是将引用放在栈中，实际存储的值是放在堆中，通过栈中的引用指向堆中存放的数据。
- 基本类型的变量是"持有"某个数值，引用类型的变量是”指向“某个对象

**注意**：栈存放的是基本数据类型(基本数据类型包括：int、short、double、long、float、boolean、char、byte；注意没有String)以及对象的引用。**栈还有一个很大的特点就是栈中的数据可以共享**。比如定义两个int类型的变量：int a = 3; int b = 3;这里a和b是一个指向int型的引用，指向"3"这个字面值。编译器先处理int a = 3;这句语句的时候，先在栈中创建一个变量为a的引用，然后查找有没字面值为3的地址，如果没有就开辟出一个存储3的地址，然后将a指向这个3对应的地址。接着处理int b = 3;，也是先创建一个变量b的引用，由于栈中已经有字面量3了，于是就把b也指向3对应的这个地址，所以a和b都指向了一个地址。当我们执行 b = 4;的时候，首先还是去栈中查找有没字面量值为4对应的地址，如果没有就开辟个，然后将b指向这个新开辟的地址。如果已经有了就直接将b指向这个地址，此时a还是指向3，但b指向4了，而且他俩不再指向同一个地址了。**当声明基本类型变量时，变量名和字面值（变量名和字面值是两个概念）均放在栈中，变量名指向字面值。**

## 2.8 引用类型

Java的引用与C++的指针在原理上是相类似的，但**Java没有指针，只有引用**。

简单的说，**引用其实就像是一个对象的名字或者别名 (alias)**，一个对象在内存中会请求一块空间来保存数据，根据对象的大小，它可能需要占用的空间大小也不等。访问对象的时候，我们不会直接是访问对象在内存中的数据，而是通过引用去访问。引用也是一种数据类型，我们可以把它想象为类似 C++ 语言中指针的东西，它指示了对象在内存中的地址——只不过我们不能够观察到这个地址究竟是什么。

如果我们定义了不止一个引用指向同一个对象，那么这些引用是不相同的，因为引用也是一种数据类型，需要一定的内存空间（stack，栈空间）来保存。但是它们的值是相同的，都指示同一个对象在内存（heap，堆空间）的中位置。

```java
String a = "hello";
String b = a;
//表示a和b是两个不同的引用，但它们的值是一样的，都指向同一个对象"hello"
```

**总结：**

- 引用是一种数据类型（保存在stack中），保存了对象在内存（heap，堆空间）中的地址，这种类型即不是我们平时所说的简单数据类型也不是类实例(对象)；
- 不同的引用可能指向同一个对象，换句话说，一个对象可以有多个引用，即该类类型的变量。

## 2.9 整数运算

计算机数字运算均是基于**补码**的。

运算规则：

- 基本四则运算法则
- 除法结果为整数 
- 除数为0时，运行将报错
- ++运算和--运算

```java
int s = 100;
System.out.println(s++); // 100
System.out.println(s);   // 101
```

- +=运算、-=运算、*=运算、/=运算、%=运算

  - 先执行等号右边的表达式，将执行结果赋值给左边的变量

  ```java
  byte b = 10;
  // b = b+5; // 编译错误，因为b+5将转为int变量
  
  b += 5;  // 编译正确，等同于 b = (byte)(b+5);
  
  byte i =0;
  i += 128; // 最终i的结果是-128，因为进行强制类型转换
  ```

  - **所以这5种赋值运算符默认会进行强制类型转换，不改变运算结果类型。假设最初这个变量的类型时byte类型，无论怎么进行追加或追减，最终该变量的数据类型还是byte类型**

- 取余运算%

**注意：运算符有优先级，不确定的加小括号()，优先级得到提升**

Java源码中经常会使用移位运算来代替乘除运算，因为移位运算的性能比乘除运算的高（PS：对于计算机而言，移位运算只是移了个位置），所以了解移位运算的计算过程对于我们阅读源码会有一定的帮助。

**原码**：第一位表示符号, 其余位表示值

**反码**：正数的反码是其本身，负数的反码是在其原码的基础上, 符号位不变，其余各个位取反

**补码**：正数的补码就是其本身，负数的补码是在其原码的基础上, 符号位不变, 其余各位取反, 最后+1. (即在反码的基础上+1)。**通常上述这种解释谁也看不懂，下面介绍一种别的说法。**举一个生活中的例子来说明这个问题：

如果说现在时针现在停在10点钟，那么什么时候时针会停在八点钟呢？简单，过去隔两个小时的时候，是八点钟。未来过十个小时的时候也是八点钟。也就是说时间正拨10小时，或是倒拨2小时都是八点钟。也就是10-2=8，而10+10=8（10+10=10+2+8=12+8=8）这个时候满12说明时针在走第二圈了，又走了8小时，所以时针正好又停在八点钟。也就是说， 10-2和10+10从另一个角度来看是等效的，它都使时针指向了八点钟。既然是等效的，那在时钟运算中，减去一个数，其实就相当于加上另外一个数（这个数与减数相加正好等于12）。这里12被称为**模**，本质上是将**溢出的部分舍去**而不改变结果。易得，**单字节(8位)运算的模为256=2^8。**故可以得到以下结论：

**负数的补码为模减去该数的绝对值的二进制**。

如-5的补码为：

-5=256-5=251=1111 1011(二进制，此时不考虑符号位，单纯二进制表示)

同样的，临界值-128也可以表示出来：

-128=256-128=128=1000 0000(二进制)



计算负数补码当然可以用上面的方法，也可以用下面我们介绍的小技巧，口算就能算出负数补码

**计算负数补码的小技巧：**首先记住该数的数据类型字节大小对应的**负数补码原点**。例如对于4位，-8是补码远点；对于8位，负数补码原点是-128；十六位可以把-32768当补码原点。**负数补码原点总是最高位是`1`，其他位是`0`**

如果我们把-8当成负数的原点。那么-5的补码是多少呢？

-5 = -8 + 3

1000（-8） +0011（3）=1011(-5)

所以可口算出-5的补码是1011



**移位运算（3种）**：

- 左移<<：丢弃左边指定位数，右边补0
- 带符号右移>>：丢弃右边指定位数，左边补上符号位
- 不带符号右移>>>：丢弃右边指定位数，左边补上0
- byte和short会先转换为int再进行移位

**结论**：对于机器而言，java中的移位运算都是对**补码**执行移位运算的，下面以-1<<1=-2为例进行讲解:

1. -1的原码：10000000 00000000 00000000 00000001
2. -1的反码：11111111 11111111 11111111 11111110
3. -1的补码：11111111 11111111 11111111 11111111
4. 执行移位操作
5. -1移位后的补码：11111111 11111111 11111111 11111110
6. -1移位后的反码：11111111 11111111 11111111 11111101
7. -1移位后的原码：10000000 00000000 00000000 00000010
8. 得到最后的原码十进制值为-2

举例如下：

```java
int n1 = 5;
int n2 = n1<<10; //5120
int n3 = n2>>11; //2
int n4 = n1<<29; //-1610612736
int n5 = n4>>29; //-3
```



**类型自动提升与强制转型**

- 在运算过程中，计算结果为较大类型的整型

```java
short s = 12;
int i = 100+s; //100为int型，计算结果自动转为int型
```

- 较大类型的整数不能直接赋值给一个较小类型的整数，相反较小类型的整数能直接赋值给较大类型的整数

```java
long a = 100L;
int b = a; //编译错误
```

- 可将结果强制转型

```java
(类型)变量或数值
long a = 100L;
int b = (int)a;
```

- 强制转型可能会丢失精度，因为int是32位，long是64位，所以如果数值大于32位会损失精度

## 2.10 浮点数运算

**浮点数运算的特点**：

- 很多浮点数无法精确表示(详情可见另一篇博客：**MySQL数据类型**)

- 计算有误差，因为浮点数无法精确表示

  ```java
  double b = 1-9.0/10; //0.09999999999999998
  ```

- 计算时如果浮点数和整数进行运算，整型可以自动提升为浮点型

**浮点数运算特殊值（三种）：**

- ```java
  double d1 = 0.0/0; //NAN，不报错
  ```

- ```java
  double d2 1.0/0; //Infinity
  ```

- ```java
  double d3 = -1.0/0; //Infinity
  ```

**强制转型**

- 强制转为整型会直接扔掉小数位

```java
int n1 = (int)12.3; //12
int n2 = (int)12.7; //12
```

- 四舍五入的技巧

```java
int n3 = (int)(12.7+0.5);
```

- 超出整型范围自动变为最大值

```java
int n4 = (int)1.2e20; //2147483647
```

## 2.11 布尔运算

- 关系运算符：> , >= , < , <= , == , !=
  - 关系运算符的运算结果一定是boolean类型，true or false

- 逻辑运算符
  - &：逻辑与（两边的算子都是true，结果才是true）
  - |：逻辑或（两边的算子只要有一个是true，结果就是true）
  - ^：逻辑异或（两边的算子结果不一样，结果就是true）
  - 逻辑非：！（取反，！false是true，！true是false）
  - **短路**与运算：&&（运算结果与&完全相同，只不过这个有短路）
  - **短路**或运算：||运算结果与|完全相同，只不过这个有短路）

逻辑运算符要求两边的算子都是boolean类型，并且逻辑运算符的最终结果也是一个boolean类型

**短路运算符：**

- 与运算&&：与运算中，如果有任何一个表达式的计算结果为false，则后面的表达式将不再计算
- 或运算||：或运算中，如果有任何一个表达式的计算结果为true，则后面的表达式将不再计算

**三元运算符：**b ? x : y

- 语法规则：布尔表达式 ？ 表示式1 ： 表达式2

- 根据条件b计算x或y，b为true计算x，b为false计算y
- x和y只计算其中一个
- x和y类型必须相同

## 2.12 字符和字符串

- 字符类型是**基本数据类型**，保存一个字符。Java使用Unicode编码，因此可将字符类型直接赋值给一个int类型。还可直接用Unicode编码表示字符类型：

```java
// '\u####',4位16进制
char c = '\u0041'; //'A'
```

- 字符串类型不是一个基本类型，是**引用类型**。String保存一个字符串。

```java
String s = "ABC";
```

- 字符串连接用+，可以连接字符串和其它数据类型

``` java
String s = "hello" + "world";
String a = "age is" + 12;
```

- 因为字符串类型是引用类型，所以字符串不可变，只能改变其引用指向的对象。

- 所有的引用类型可以指向空值null，表示不指向任何对象。空值null和空字符串""是不一样的，注意区分。

```java
String s = null;
```

## 2.13 数组类型

当有一组类型相同的变量时，可以用数组表示：

- 数组类型是：**类型[]**
- 数组初始化用**new int[数组长度]**
- 数组所有元素初始化为默认值
- 数组创建后大小不可改变
- 数组索引从0开始

```java
public class hello{
    public static void main(String[] args){
        int[] ns = new int[5];
        ns[0] = 1;
        ns[1] = 2;
        ns[2] = 3;
        ns[3] = 4;
        ns[4] = 5;
    }
}
```

- 用数组变量.length获取数组大小

```java
int[] ns = new int[5];
System.out.println(ns.length);
```

- **数组变量是引用类型**

- 可以指定初始化的元素，由编译器自动推算数组大小

```java
int[] ns = new int[] {1,2,3,4,5};
//可进一步简写
int[] ns = {1,2,3,4,5};
```

- 数组变量是引用类型，数组大小不可变，可指向不同的数组对象
- 数组元素是值类型（如int[]）或引用类型（如String[]）



## 2.14 Java对日期的处理

```java
import java.util.Date;
public class Time {
    public static void main(String[] args) {
        // 获取系统当前时间
        Date noWTime = new Date();
        // Date类重写了toString方法
        // Fri May 08 15:45:48 GMT+08:00 2020
        System.out.println(nowTime);
    }
}
```

**日期可以格式化么**？将日期类型Date，按照指定的格式进行转换：Date --> 转换成具有一定格式的日期字符串 

SimpleDateFormat是java.text包下的，专门负责日期格式化

```java
import java.util.Date;
import java.text.SimpleDateFormat;
public class Array {
    public static void main(String[] args) {
        Date t = new Date();
        //Fri May 08 15:51:55 GMT+08:00 2020
        System.out.println(t);
        SimpleDateFormat s = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        String tStr = s.format(t);
        //2020-05-08 15:51:55 319
        System.out.println(tStr);
    }
}
```

**假设有一个日期字符串String，怎么转换成Date类型？**

```java
import java.util.Date;
import java.text.SimpleDateFormat;

public class Array {
    public static void main(String[] args) throws Exception{
        String time = "2008-08-08 08:08:08 888";
        // 格式要和time的格式相同
        SimpleDateFormat s2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        // 注意抛出异常，否则会报错
        Date dateTime = s2.parse(time);
        System.out.println(dateTime);
    }
}
```



**获取自1970年1月1日 00:00:00 000到当前系统时间的总毫秒数**

```java
public class Array {
    public static void main(String[] args){
        long n = System.currentTimeMillis();
        System.out.println(n); //1588924995077
    }
}
```

这个功能可以用来计算方法执行时间

```java
public class Array {
    public static void main(String[] args){
        long a = System.currentTimeMillis();
        print();
        long b = System.currentTimeMillis();
        System.out.println("耗费时长"+(b-a)+"毫秒");
    }

    public static void print(){
        for(int i=0;i<1000;i++){
            System.out.println(i);
        }
    }
}
```



**通过毫秒构建Date对象**

上面介绍了Date类的无参构造函数；这里还有一个有参的构造函数

```java
// Date(long date):date指自格林威治1970年1月1日 00:00:00以来的毫秒数
import java.util.Date;
import java.text.SimpleDateFormat;

public class Array {
    public static void main(String[] args){
        Date s = new Date(1);
        SimpleDateFormat date = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss SSS");
        String dateStr = date.format(s);
        //1970-01-01 08:00:00 001(北京是东八区)
        System.out.println(dateStr);
    }
}
```



## 2.15 数字格式化

#：代表任意数字

，：代表千分位

.：代表小数点

0：不够时补0

```java
import java.text.DecimalFormat;
public class Array {
    public static void main(String[] args){
        //保留两位小数，加入千分位
        DecimalFormat df = new DecimalFormat("###,###.##");
        String s = df.format(1234.456);
        System.out.println(s); //1,234.46
        
        DecimalFormat df2 = new DecimalFormat("###,###.0000");
        String s2 = df2.format(1234.45);
        System.out.println(s2); //1,234.4500
    }
```



## 2.16 BigDecimal

BigDecimal属于大数据类型，精度极高，不属于基本数据类型，属于引用数据类型

常用于财务软件当中，财务软件当中double 的精度是不够的

```java
import java.math.BigDecimal;
public class Array {
    public static void main(String[] args){
        // 不是普通的100，是精度极高的100
        BigDecimal a = new BigDecimal(100);
        BigDecimal b = new BigDecimal(200);
        // 不能直接使用+号进行相加
        BigDecimal c = a.add(b);
        System.out.println(c); // 300
    }
}

```



## 2.17 随机数

```java
import java.util.Random;
public class Array {
    public static void main(String[] args){
        Random random = new Random();
        //随机产生一个int范围类型的随机数
        int num = random.nextInt();
        System.out.println(num);
        
        // 产生[0,100]之间的随机数，不能产生101
        int x = random.nextInt(100);
        System.out.println(x);
    }
}
```



**生成5个不重复的随机数**

```java
import java.util.Random;
import java.util.Arrays;
public class Array {
    public static void main(String[] args){
        Random random = new Random();
        int[] elements = new int[5];
        elements[0] = random.nextInt(100);
        int index = 1;
        while(index<=4){
            int tmp = random.nextInt(100);
            for(int i=0;i<index;i++){
                if(elements[i]==tmp)
                    break;
                if(i==index-1 && elements[i]!=tmp)
                    elements[index++] = tmp;
            }
        }
        System.out.println(Arrays.toString(elements));
    }
}
```



## 2.18 枚举类型

枚举：一枚一枚可以列举出来的，才建议使用枚举类型

枚举编译后也是生成class文件

枚举是一种引用数据类型

枚举中的每一个值可以看做是常量

**方法中的计算结果只有两种情况的，建议使用boolean；结果超过两种并且可列举，使用枚举类型**

```java
public class Array {
    public static void main(String[] args){
        Result r = divide(10,0);
        System.out.println(r==Result.SUCCESS ? "计算成功" : "计算失败"); // 计算失败
    }

    public static Result divide(int a,int b){
        try{
            int c = a/b;
            return Result.SUCCESS;
        }catch(Exception e){
            return Result.FAIL;
        }
    }
}

enum Result{
    // SUCCESS是枚举类型中的一个值
   	// FAIL是枚举类型中的一个值
    SUCCESS,FAIL
}
```



# 3. 流程控制

## 3.1 输入和输出

**输出**：

- 输出并换行

```java
System.out.println()
```

- 输出但不换行

```java
System.out.print()
```

**输入**:

- 导入java.util.Scanner
- 创建Scanner并传入System.in
- 使用scanner.nextLine()读字符串
- 使用scanner.nextInt()读Int整数
- 使用scanner.nextDouble()读长整型
- ......

```java
import java.util.Scanner;

public class hello{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        System.out.println("Input your name:");
        String name = scanner.nextLine();
        System.out.println("Input your age:");
        int age = scanner.nextInt();
    }
}
```

**格式化输出**：

- 格式化输出使用System.out.printf()
- 使用占位符%###

```java
System.out.printf("%s is %d years old\n","Bob",12);
```

常用占位符：

| 占位符 |         含义         |
| :----: | :------------------: |
|   %d   |         整数         |
|   %x   |     十六进制整数     |
|   %f   |        浮点数        |
|   %s   |        字符串        |
|   %%   |    百分号字符本身    |
|   %e   | 科学计数法表示浮点数 |
|   %c   |         字符         |
|   %b   |        布尔值        |

```java
public class hello{
    public static void main(String[] args){
        double d = 3.1415926;
        System.out.println(d);
        System.out.printf("PI = %.2f\n",d);
        System.out.printf("PI = %7.2f\n",d);
    }
}
```

运行结果如下：

```java
3.1415926
PI = 3.14
PI =    3.14 //3前面补充了三个空格 
```

**格式标识符解释**

**%7.2f**：  

7为域宽度.输出的浮点数条目宽度至少为7,包括小数点和小数点后两位数字.这样,给小数点前分配了4位数字.
如果该条目小数点前的位数小于4,就在数字前面加空格.
如果该条目小数点前的位数大于4,则自动增加宽度.

2为精度().即想要输出的小数点的长度.

f为转换码  



**指定宽度和精度的例子**

| 举例   | 输出                                                         |
| ------ | ------------------------------------------------------------ |
| %5c    | 输出字符并在这个字符条目前面加4个空格                        |
| %6b    | 输出字符并在这个字符条目前面加4个空格                        |
| %5d    | 输出整个条目,宽度至少为5.如果该条目的数字位数小于5,就在数字前面加空格.如果该条目的位数大于5,则自动增加宽度 |
| %10.2f | 输出的浮点条目宽度至少为10,包括小数点、和小数点后两位数字.这样,给小数点前分配了7位数字. 如果该条目小数点前的位数小于7,就在数字前面加空格. 如果该条目小数点前的位数大于7,则自动增加宽度. |
| %10.2e | 输出的浮点条目的宽度至少为10,包括小数点、小数点后面两位数字和指数部分(包括e、正负号和指数数字).如果按科学计数法显示的数字位数小于10,就给数字前加空格 |
| %12s   | 输出的字符串至少为12个字符.如果该字符串条目小于12个字符,就在该字符串前加空格.如果该字符串条目多余12个字符,则自动增加宽度 |

**注意的问题**：

- 默认情况下,输出是右对齐的（即如果宽度不够，在左边补齐空格）.可以在格式标识符中放一个符号(-),表明该条目在特定区域中的输出是左对齐的.

```java
System.out.printf("%8.4f",1.12);
System.out.printf("%-8.4f",1.12);

// 输出
  1.1200
1.1200
```

- 使用符号%来标记格式标识符,要在格式字符串里输出直接量%,需要使用%%

- 如果在补齐时不希望以空格进行补齐，而希望以0进行补齐，如下所示：

```java
System.out.printf("PI = %08d\n",1234);

// 输出
00001234
```

- 如果想显示符号位，如下所示：

```javascript
System.out.printf("PI = %+8d\n",1234);
// 无论正负都可以用上述形式显示符号位
// 输出
   +1234 //符号也占一位
```

- 如何控制输出顺序

```java
// #$表示第几个变量
System.out.printf("%2$d, %1$d",13,14);
System.out.printf("%2$s, %1$s","a","b");

// 输出
14, 13
b, a
```



## 3.2 引用类型判断是否相等

- 判断两个引用是否指向堆中的同一个实例，用"=="

```java
String a = "hello";
String b = "world";
Sytsem.out.println(a==b);
```

- 判断两个引用指向的实例是否相等，用equals。如果变量为null，调用equals()会报错，利用短路运算符&&解决。

```java
String a = "hello";
String b = "world";
if(a!=null && a.equals(b))
    System.out.println("yes");
```

## 3.3 if语句

语法结构：四种编写方式

1. if(布尔表达式){java语句}
2. if(布尔表达式){java语句} else{java语句}
3. if(布尔表达式){java语句} elseif(布尔表达式){java语句} else if{java语句} ... 
4. if(布尔表达式){java语句} elseif(布尔表达式){java语句} else if{java语句} else{java语句}

- if语句最多只能执行一个分支（0个或1个）
- 以上的第二种和第四种方式都带有else分支，这两种方式可以保证会有一条分支执行。第一种和第三种可能一个分支也不执行
- 可以嵌套使用
- if语句分支中只有一条java语句，大括号可以省略，不推荐

## 3.4 Switch语句

```java
int opt = 2;
switch(opt){
    case 1:
        System.out.println("1");
        break;
    case 2:
        System.out.println("2");
        break;
    case 3:
        System.out.println("3");
        break;
    default:
        System.out.println("none");
}
```

- **switch后面的括号里只能写int或string类型的字面值或变量**
- 注意case语句没有{}
- case语句具有"**穿透性**"，如果不加break会导致意想不到的结果。当opt=2，如果不写break导致case2、case3和default顺序执行（**case3和default不用管是否匹配**）
- 如果都没有匹配，则执行default
- switch和case后面的计算结果必须是整形、字符串（字符串比较内容相等）。从JDK7开始，byte、short和char也可以，因为可以自动类型转换
- case可以合并

```java
int i = 1;
switch(i){
	case 1: case2: case3: // i=1、2、3都可以执行下述语句
		System.out.println("Test");
}
```

**如何避免漏写break和default？**

- 打开eclipse中的窗口->首选项

{% asset_img 微信图片_20200310135053.png %}

- Java->编译器->错误/警告->可能的编程问题，将"switch缺少default语句"和"switch case跳转"修改为警告：

{% asset_img 微信图片_20200310135337.png %}

这样以后如果忘了写break或default时会有警告

## 3.5 While循环

- while循环首先判断条件
- 条件满足时循环
- 条件不满足时退出
- 可能一次都不循环
- 注意逻辑，避免无限死循环

```java
int sum = 0;
int n = 1;
while(n<10){
    sum = sum+n;
    n++;
}
```

## 3.6 do-while循环

- do-while先执行循环，再判断条件
- 条件满足时继续循环
- 条件不满足时退出
- 至少循环一次

```java
int sum = 0;
int n = 1;
do{
    sum += n;
    n++;
} while(n<10);
```

## 3.7 for循环

for循环分别设置：

- 计数器初始值
- 循环前检测条件
- 每次循环后如何更新计数器

```java 
int[] ns = {1,2,3,4,5};
for(int i=0;i<ns.length;i++){
    System.out.println(ns[i]);
}
```

- **for里面的三个语句可以都没有**
- 当嵌套for循环时，内层循环和外层循环的变量不能重名
- 初始化计数器总是被执行
- 可能循环0次



**for each循环**

JDK5.0之后的新特性：增强for循环

```java
int[] ns = {1,2,3,4,5};
for(int n:ns){
    System.out.println(n);
}
```

- for each循环可以更简单的遍历数组
- for each循环能够遍历数组和"可迭代"数据类型，包括List、Map等
- for each循环无法指定遍历顺序，只能从前往后单向循环
- **for each循环无法获取数组索引，因此无法同时遍历多个数组**。只能通过for循环

```java
int[] ns1 = {1,2,3,4,5};
int[] ns2 = {6,7,8,9,10};
for(int i=0;i<ns1.length;i++){
    ns2[i] = ns1[i]*ns1[i];
}
```

## 3.8 break和continue

在循环中，可以使用break语句和continue语句。

### break跳出循环

- 循环过程中，可以使用break语句跳出循环
- 如果有多层循环嵌套，break语句总是跳出最近的一层循环。
- break通常配合if，在满足条件时提前结束循环

### continue提前结束当前循环

- continue可提前结束本轮循环，直接继续下次循环
- continue通常配合if，在满足条件时提前结束本轮循环

# 数组操作

## 遍历数组

- for循环可以遍历数组
- for(;;)循环通过下标遍历数组
- for each循环直接遍历数组元素



直接打印数组变量，得到的是数组在JVM中的引用地址。for each循环打印也很麻烦。Arrays.toString()可以快速打印数组内容

```java
import java.util.Arrays;

int[] ns = {1,1,2,3,5,8};
System.out.println(Arrays.toString(ns));
```

## 数组排序

有两种方法：

- 自己写排序算法，比如冒泡排序、快排等
- 使用JDK的Arrays.sort()排序。

```java
import java.util.Arrays;

int[] ns = {28,12,89,73,65};
Arrays.sort(ns);
System.out.println(Arrays.toString(ns)); // Arrays重写了toString方法，输出数组
```

- 对数组排序修改了数组本身

## 多维数组

### 二维数组

- 二维数组就是数组的数组

```java
int[][] ns = {
    {1,2,3,4},
    {5,6,7,8},
    {9,10,11,12}
};
```

二维数组的内存空间分配如图所示：

{% asset_img 微信图片_20200312103806.png %}

数组变量分配在栈内存中，其指向一个三维数组，该三维分配在堆内存上。每一维数组元素由指向一个一维数组，同样分配在堆内存上。

- 访问二维数组元素使用array[rows] [cols]
- 二维数组每个数组元素长度不要求相同

{% asset_img 微信图片_20200312105221.png %}

### 三维数组

与二维数组类似，内存分配如图所示

{% asset_img 微信图片_20200312105351.png %}

```java
int[][][] ns = {
    {
        {1,2,3},
        {4,5,6},
        {7,8,9}
    },
    {
        {10,11},
        {12,13}
    },
    {
        {14,15,16},
        {17,18}
    }
};
```

其余多维数组类似

