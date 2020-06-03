---
title: String类
date: 2020-05-07 09:33:58
tags:
- Java
categories:
- Java
mathjax: true
---

关于Java JDK中内置的一个类：**java.lang.String**

- String表示字符串类型，属于引用数据类型
- **重点：在Java中使用双引号括起来的都是String对象**
- Java规定，双引号括起来的字符串是不可变的。也就是说"abc"自出生到死亡不可变，不能变成"abcd"
- 在JDK当中双引号括起来的字符串，例如"abc"都是直接存储在**方法区的常量池**中。为什么这么设计呢？
  - 因为字符串在实际开发中使用太频繁，为了执行效率将其放在方法区的常量池中
- **垃圾回收器不会释放常量池中的对象**



通常有两种方式创建String对象：

- 第一种

```java
String s1 = "abc";
```

采用这种方式时，"abc"这个String对象存储在方法区的字符串常量池中，在栈中创建了一个s1引用，该引用保存这个String对象的内存地址，指向常量池中的该对象（**注意：堆内存中不会创建任何对象**）。如果此时又有String q = "abc"；那么常量池中不会新建"abc"字符串对象，q依旧指向上面的"abc"对象，q和s1指向的是同一个字符串对象。**这个时候可以用"=="比较两个字符串对象是否相等**



- 第二种

```java
String s2 = new String("abc")
```

采用这种方式时，"abc"这个String对象依然存储在方法区的字符串常量池中，同时在堆中创建了一个对象，该对象中保存着这个常量池中的String对象的内存地址。同时在栈中创建了一个s2引用，该引用保存这个堆内存中对象的内存地址（**注意：堆内存中会创建对象**）**。这个时候就不能用"=="判断两个字符串对象是否相等，要用equals方法 。**

如果此时又有String e = new String("abc")；那么常量池中不会增加字符串对象，但是堆中会多一个对象，该对象中存储着"abc"这个String对象的内存地址。e指向堆内存中新创建的对象，e和s2指向的是不同的对象



**String常用的构造方法**

```java
// 创建String对象最常用的方法
String s1 = "abc";

// 常用的构造方法
// 第一种，参数是byte数组
byte[] bytes = {97,98,99}; //97是a，98是b，99是c
String s2 = new String(bytes);
System.out.println(s2); // abc

// 第二种
byte[] bytes = {97,98,99}; //97是a，98是b，99是c
String s3 = new String(bytes,1,2); // 1是数组起始下标，2是长度
System.out.println(s3); // bc

// 第三种
char[] chars = {'a','b','c'};
String s4 = new String(chars);
System.out.println(s4); // abc

// 第四种
char[] chars = {'a','b','c'};
String s5 = new String(chars,1,2); // 1是数组起始下标，2是长度
System.out.println(s5); // bc

// 第五种
String s6 = new String("hello,world");
System.out.println(s6); // hello,world
```



## String中的常用方法

- charAt方法

```java
// char charAt(int index)，返回指定索引的字符
char c = "hello".charAt(1);
System.out.println(c); // e
```

- compareTo方法：比较字符串字典序大小关系，从左到右依次进行比较，如果相等就往后；如果不等就输出1或-1；如果全部都相等就输出0

```java
// int compareTo(String anotherString)
int result1 = "abc".compareTo("abc");
System.out.println(result1); // 0，前后相等

int result2 = "abcd".compareTo("abce");
System.out.println(result2); // -1，前小后大

int result3 = "abce".compareTo("abcd");
System.out.println(result3); // 1，前大后小

int result4 = "xyz".compareTo("yxz");
System.out.println(result4); // -1，前小后大
```

- contains方法：判断前面的字符串是否包含后面的字符串

```java
// boolean contains(String s)
System.out.println("helloworld").contains("world"); // true
```

- endsWith方法：判断当前字符串是否以某个字符串结尾

```java
// boolean endsWith(charSequence s)
System.out.println("helloworld").endsWith("world"); // true
```

- endsWith方法：判断当前字符串是否以某个字符串开始
- equalsIgnoreCase(String anotherString)：忽略两个字符串大小写，判断是否相等

```java
// boolean equalsIgnoreCase(String anotherString)
System.out.println("ABc".equalsIgnoreCase("abC"); // true
```

- getBytes方法：将字符串对象转换成byte数组

```java
byte[] bytes = "abc".getBytes();
```

- indexOf方法：判断某个子字符串在当前字符串第一次出现处的索引，返回值为int

```java
System.out.println("OracleJavaC++Java".indexOf("Java");  // 6
```

- lastIndexOf方法：判断某个子字符串在当前字符串中最后一次出现的索引

```java
System.out.println("OracleJavaC++Java".indexOf("Java");  // 13
```

- isEmpty方法：判断某个字符串是否为空，返回值boolean

```java
String s = "";
System.out.println(s.isEmpty()); // true
```

- length方法：返回字符串长度，返回值int。**注意数组中是length属性获得数组长度**

```java
System.out.println("abc".length()); //3 
```

- replace方法：替换。还有个replaceAll，和这个差不多，只不过target支持正则表达式

```java
// String replace(String target,String replacement)
String s = "http://".replace("http","https");
```

- split方法：拆分字符串

```java
// String[] split(String regex)
String[] s = "1990-01-01".split("-"); // {"1990","01","01"}
```

- substring方法：截取字符串

```java
// String substring(int beginIndex)，参数是起始下标
String s = OracleJava.substring(6); // Java

// String substring(int beginIndex，int endIndex)，参数是起始下标和结束下标,左闭右开[beginIndex,endIndex)
// 包括beginIndex，不包括endIndex
String s = OracleJava.substring(6,8); // Ja
```

- toCharArray方法：将一个字符串转换成char数组

```java
// char[] toCharArray()
char[] chars = "hello".toCharArray();//{'h','e','l','l','o'}
```

- toLowerCase方法：转换成小写

```java
String s = "ABC".toLoweCase(); // abc
```

- toUpperCase方法：转换成大写

- trim方法：去除字符串前后空格，不能去除中间的空格

```java
String s = "    hello world   ".trim(); // hello world
```

- valueOf方法：**String类中唯一的静态方法，将非字符串转换成字符串**。如果方法中传入的是引用数据类型的对象，那么会调用该对象的toString方法转换成字符串

```java
String s = String.valueOf(true);
System.out.println(s); // "true"
// 本质上System.out.println这个方法在输出任何数据的时候都是先转换成字符串再输出，即会先调用valueOf方法
```





## StringBuffer

在实际开发中，如果需要进行字符串频繁拼接，会有什么问题？

- 因为Java字符串是不可变的，每一次拼接都会产生新字符串，这样会占用大量的方法区内存，造成内存空间的浪费。

**此时就要用的StringBuffer或StringBulider**

```java
public class Test{
    public static void main(String[] args){
        StringBuffer stringBuffer = new StringBuffer();
        // 拼接字符串调用append方法，可以追加任何类型的数据
        // append方法底层在进行追加的时候，如果byte数组满了，会自动扩容
        stringBuffer.append("hello");
        stringBuffer.append(true);
        stringBuffer.append(3.14);
        stringBuffer.append(100);
        System.out.println(stringBuffer.toString());// "hellotrue3.14100"
    }
}
```

StringBuffer底层实际上是一个byte数组（字符串缓冲区对象），往StringBuffer中放字符串，实际上是放到byte数组当中了，StringBuffer的默认初始化容量是16个字符。String底层也是一个byte数组，但是**由final修饰**，所以String字符串是不可变的



**如何优化StringBuffer？**

- 在创建StringBuffer的时候尽可能给定一个初始化容量，减少底层数组的扩容次数。如何给定？

```java
StringBuffer stringBuffer = new StringBuffer(100); // 容量为100个字符
```



## StringBuilder

使用StringBuilder也能完成字符串拼接

```java
public class Test{
    public static void main(String[] args){
        StringBuilder stringBuilder = new StringBuilder();
        // 拼接字符串调用append方法，可以追加任何类型的数据
        // append方法底层在进行追加的时候，如果byte数组满了，会自动扩容
        stringBuilder.append("hello");
        stringBuilder.append(true);
        stringBuilder.append(3.14);
        stringBuilder.append(100);
        System.out.println(stringBuilder.toString());// "hellotrue3.14100"
    }
}
```



**StringBuffer和StringBuilder的区别？**

- StringBuffer中的方法都有synchronized关键字修饰，表示StringBuffer方法在多线程环境下运行是安全的；StringBuilder中的方法都没有synchronized关键字修饰，表示非线程安全