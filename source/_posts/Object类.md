---
title: Object类
date: 2020-05-03 09:58:11
tags:
- Java
categories:
- Java
mathjax: true
---

**什么是API？**

应用程序编程接口（Application Program Interface）

整个JDK的类库就是一个JavaSE的API

每个API都会配置一套API帮助文档



## toString方法

源码如下：

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

- 源代码上toString方法的默认实现是：**类名@对象的内存地址转换为十六进制的形式**
- toString方法的作用：toString方法的设计目的是：**通过调用这个方法可以将一个java对象转换成字符串表示形式**
- 建议所有的子类都重写toString方法，因为默认的实现不简洁不易阅读（**toString方法以后在写的时候一般都要重写**）

```java
package test;

public class test {
    public static void main(String[] args) {
        myTime m = new myTime(1970,1,1);
        String s = m.toString();
        System.out.println(s); // test.myTime@b4c966a
        // 输出引用的时候，会自动调用toString方法
        System.out.println(m); // test.myTime@b4c966a
    }
}

class myTime{
    int year;
    int month;
    int day;
    public myTime(){}
    public myTime(int year,int month,int day){
        this.year = year;
        this.month = month;
        this.day = day;
    }
}
```



## equals方法

源码如下：

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

- equals方法的目的是**判断两个对象是否相等**
- 判断两个基本数据类型的数据是否相等直接使用“==”即可；**判断两个java对象对象是否相等不能直接用双等号，如果使用双等号判断的是两个对象的内存地址是否相等，而不是两个对象是否相等**
- 在Object类中默认的equals方法采用双等号判断两个对象是否相等，所以该默认equals方法不够用，需要在子类中重写equals方法（String也是引用数据类型的，但该类中重写了equals方法，所以比较的是字符串内容是否相等）
- **注意：重写equals方法要彻底（每个自定义类都要重写equals方法）**
- 如何重写equals方法（**自己定，你认为什么时候对象相等就怎么写**）

```java
class myTime{
    int year;
    int month;
    int day;
    public myTime(){}
    public myTime(int year,int month,int day){
        this.year = year;
        this.month = month;
        this.day = day;
    }
    
    @Override
    public boolean equals(Object obj) {
        if(obj==null || !(obj instanceof myTime)) return false;
        // 如果两个对象的内存地址相等，那么一定表示两个对象相等
        if(this == obj) return true;
        
        myTime t = (myTime)obj;
        return this.year == t.year && this.month == t.month && this.day == t.day
    }
}
```

**在IntelliJ IDEA中可以自动生成重写的toString和equals方法（用Alt+Insert快捷键插入）**



## String类重写了toString方法和equals方法

String类中的源码：

```java
@Override
// 用于返回String的字符串值
public String toString() {
    return this;
}

public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

- **比较两个字符串不能用双等号，必须调用equals方法**。String类已经重写了equals方法和toString方法。**重写后的equals方法用来比较两个字符串内容是否相同；重写后的toString方法用来输出字符串值**



## finalize方法

源码如下：

```java
protected void finalize() throws Throwable { }
```

- finalize方法只有一个方法体，里面没有代码，而且这个方法是protected修饰的
- 这个方法**不需要程序员手动调用**，JVM垃圾回收器负责调用这个方法。equals和toString方法需要程序员手动调用
- finalize方法的执行时机：当一个Java对象即将被垃圾回收器回收的时候，垃圾回收器负责调用
- finalize方法实际上是SUN公司为Java程序员准备的一个时机（垃圾销毁时机）。**如果希望在对象销毁时机执行一段代码的话，这段代码要写到finalize方法中（有点类似于静态代码块）**
- 有点像C++中的析构函数
- 可以重写该方法，在对象被回收的时候，执行方法体
- 当然了，对象也有可能不被回收（因为垃圾回收器的启动是有条件的，如果垃圾太少可能不启动），所以finalize方法可能不被调用。有一段代码可以建议垃圾回收器启动（但可能不听话，有可能启动，也可能不启动）

```java
myTime r = new myTime();
r = null;
System,gc(); // 建议垃圾回收器启动
```



## hashCode方法

源码如下：

```java
public native int hashCode();
```

- 带有native关键字，底层调用C++程序
- hashCode方法返回的是哈希吗，实际上就是一个java对象的内存地址，经过哈希算法，得到的一个值。**所以hashCode方法的执行结果可以等同看成一个java对象的内存地址**

```java
public class test{
    public static void main(String[] args) {
        myTime t = new myTime();
        System.out.println(t.hashCode()); // 793589513
    }
}
```

