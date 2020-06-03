---
title: 从JVM角度看Java多态
date: 2020-03-15 15:36:32
tags:
- Java
categories:
- Java
mathjax: true
---

多态举例如下：

```java
public class Father{ // 父类
    protected int age;
    public Father(){
        age = 40;
    }
    void eat(){
        System.out.println("父亲在吃饭");
    }
}

public class Child extends Father{  // 子类
    protected int age;
    public Child(){
        age = 18;
    }
    void eat(){
        System.out.println("孩子在吃饭");
    }
    void play(){
        System.out.println("孩子在打CS");
    }
}

public class Test {
    public static void main(String[] args) {
        Father f = new Child(); // 父类引用指向子类对象，多态实现形式
        f.eat(); // 调用子类eat()
        //f.play(); 编译错误
        System.out.println("年龄："+f.age ); // 父类age
    }
}
```



下面从JVM的角度解释上面这种现象，从下面这句代码切入

```java
Father f = new Child();
```

这句代码首先会执行new Child()，在堆中分配一个对象。当然在创建Child类的实例前，先要通过JVM的类加载器将Child类对应的class文件加载到JVM中，然后JVM根据class文件中的字节流产生一个表示class文件的**类型信息结构体**。

这个表示class文件的类型信息结构体大概由以下几部分构成：

- 常量池（较为复杂，存放该类型所用到的常量的有序集合，包括直接常量（字符串、整数、浮点数）和对其他类的字段、方法的符号引用）
- 类变量（静态变量，**只包含本类所定义的，不包含继承自父类的，下面字段信息、方法信息类似**）
- 字段信息
- 方法信息
- 类型信息
- 指向类加载器的引用
- 指向class实例的引用
- 方法表
- 父类信息引用

之后，JVM会根据上面这个结构体生成一个叫做**虚方法表(vtable)**的东西。这个方法表是实现Java多态的一个关键。方法表中包含的是实例方法（**就是相对于静态方法而言的，用对象访问的那些方法，即不包含static、final和private修饰的方法**）的直接引用，也就是说通过这个方法表就能够访问到该类的实例方法。

而且，这些实例方法**不仅包括本类的方法，还包括其父类的实例方法，以及父类的父类的实例方法（就是一直到Object）**。

方法表中的这些直接引用会指向到JVM中表示类型信息的那个结构体（就是上面那个结构体）的相应的方法信息（就是上面结构体中第4块的某个位置），当然这只是本类的方法，方法表中还有父类的方法，相应地指向父类类型信息结构体的具体位置。如图所示

{% asset_img 823435-20170514185548363-901763715.png %}

上面提到过，**方法表中不仅包括本类的方法，还包括父类的方法**，方法表值这样产生的，以Child类的方法表为例：

- 首先方法表中，会产生指向继承自Object类的方法的引用，这些包括指向toString的和指向equals的，当然Object中还包括很多方法，这里就不写了

- 然后方法表中产生指向继承自Parent类的方法的引用，这包括eat，

- 最后产生指向本类的方法的引用。

这里需要注意的一点是，当Child类的方法表产生指向Parent类中的方法的引用时，会有一个指向eat方法的引用，最后产生指向本类的方法的引用时，也有一个指向eat的引用，这时候，新的数据会覆盖原有的数据，也就是说原来指向Parent.eat的那个引用会被替换成指向Child.eat的引用（占据原来表中的位置）。所以我们看到在Child类的方法表中指向的是Child.eat而Parent类的方法表中指向的是Parent.eat。子类的方法表中就没有指向Parent.eat的引用了。(**重写的底层实现**)

而且还要注意一个特点就是，Parent和Child的方法表中，指向eat的引用在表中的偏移量是一样的，都是第三个位置。（这是因为子类eat方法覆盖掉了父类eat方法，占据了原来父类eat方法的引用在表中的位置）

**这里再多说一句，表示类型信息的结构体（非方法表）中的方法信息，只包含本类特有的，或者是重写的方法信息，没有父类的方法信息。**



这里介绍一下方法表的两个特点：

方法表本质上是一个数组，每个数组元素指向一个当前类及其祖先类中非私有的实例方法。

**方法表满足两个性质：**

- 子类方法表中包含父类方法表中的所有方法
- 子类方法在方法表中的索引值，与它所重写的父类方法的索引值相同。（即如果子类方法表和父类方法表中同时含有一个函数，那么该函数在各自方法表中的索引值相同）



了解了方法区的结构后，再来看堆中对象的结构

{% asset_img 823435-20170514193200941-1234924738.png %}

**从图中可以看出，堆中的实例对象不仅包含本类实例变量，也包含父类实例变量，分配不同的内存，不存在冲突。本类实例变量分配在本类实例变量区，父类实例变量分配在父类实例变量区。子类实例变量区和父类实例变量区不冲突不重合，都存在于该实例对象堆内存空间中**。

**由于类的实例变量属于类对象实例，所以分配在堆内存中。静态变量属于类，不属于类实例，所以分配在方法区中。**



接下来是栈区，产生Father类型的引用，这个引用指向堆区中的Child类的实例。

这里需要解释一下Father f的含义，我们知道f表示一个引用，这个引用指向堆中的Child类的实例，说白了就是一个地址(其实Java中没有地址，因为地址不安全)，这个地址指向堆中的Child的类的实例

 



## 下面探讨一下如何调用方法及变量？

### **1. 调用static方法、final方法和private方法**

```java
class Father{
　　public static void f1(){
　　System.out.println("Father— f1()"); 
  } 
}

public class StaticCall{
　　public static void main(){
　　Father.f1(); //调用静态方法
　　}
}
```

上面的源代码中执行方法调用的语句(Father.f1())被编译器编译成了一条指令：**invokestatic #13**。我们看看JVM是如何处理这条指令：

- 指令中的#13指的是StaticCall类的常量池中第13个常量表的索引项。这个常量表(CONSTATN_Methodref_info) 记录的是方法f1信息的符号引用(包括f1所在的类名，方法名和返回类型)。JVM会首先根据这个符号引用找到方法f1所在的类的全限定名: Father;
- 紧接着JVM会加载、链接和初始化Father类;
- 然后在Father类所在的**方法区**中找到f1()方法的直接地址，并将这个直接地址记录到StaticCall类的常量池索引为13的常量表中。这个过程叫常量池解析 ，以后再次调用Father.f1()时，将直接找到f1方法的字节码;
- 完成了StaticCall类常量池索引项13的常量表的解析之后，JVM就可以调用f1()方法，并开始解释执行f1()方法中的指令了。

通过上面的过程，我们发现经过常量池解析之后，JVM就能够确定要调用的f1()方法具体在内存的什么位置上了。实际上，这个信息在编译阶段就已经在StaticCall类的常量池中记录了下来。这种在编译阶段就能够确定调用哪个方法的方式，我们叫做**静态绑定机制** 。



### 2. 调用实例方法

```java
class Father{
　　public void f1(){
　　System.out.println("father-f1()"); 
  } 
    public void f1(int i){
　　System.out.println("father-f1() para-int "+i);
　　} 
} //被调用的子类

class Son extends Father{
　　public void f1(){
　　//覆盖父类的方法
　　System.out.println("Son-f1()");
  } 
} //调用方法

public class AutoCall{
    public static void main(String[] args){
        Father father=new Son();
        father.f1();
    }
}
```

对于上面的源代码，编译器首先会把main方法编译成下面的字节码指令：

```java 
0 new Son [13] //在堆中开辟一个Son对象的内存空间，并将对象引用压入操作数栈
3 dup
4 invokespecial #7 [15] 
7 astore_1 //弹出操作数栈的Son对象引用压入局部变量1中
8 aload_1 //取出局部变量1中的对象引用压入操作数栈
9 invokevirtual #15 //调用f1()方法
12 return
```

其中**invokevirtua**l指令的详细调用过程是这样的：

- invokevirtual指令中的#15指的是AutoCall类的常量池中第15个常量表的索引项。这个常量表(CONSTATN_Methodref_info) 记录的是方法f1信息的符号引用(包括f1所在的类名，方法名和返回类型)。JVM会首先根据这个符号引用找到调用方法f1的类的全限定名:Father。这是因为调用方法f1的类的对象father声明为Father类型。
- 在Father类型的**虚方法表**中查找方法f1，如果找到，则将方法f1在方法表中的索引项记录到AutoCall类的常量池中第15个常量表中(常量池解析 )。这里有一点要注意：如果Father类型方法表中没有方法f1，那么即使Son类型中方法表有，编译的时候也通过不了。因为调用方法f1的类的对象father的声明为Father类型。
- 在调用invokevirtual指令前有一个aload_1指令，它会将开始创建在堆中的Son对象的引用压入操作数栈。然后invokevirtual指令会根据这个Son对象的引用首先找到堆中的Son对象，然后进一步找到Son对象所属类型的方法表。
- 这是通过第(2)步中解析完成的#15常量表中的方法表的索引项11，可以定位到Son类型方法表中的方法f1()，然后通过直接地址找到该方法字节码所在的内存空间

上述即为多态的实现机制，即**动态绑定**。



### 3. 调用变量

```java
// Father 父类
public class Father{
    int age = 10;
}

public class Son extends Father{
    int age = 20;
}
// Son 子类
Father f = new Son();
System.out.println(f.age); // 10
```

这里主要涉及到 Java里面一个**字段隐藏**的概念。父类和子类定义了一个同名的字段，不会报错。**但对于同一个对象，用父类的引用去取值，会取到父类的字段；用子类的引用去取值会取到子类字段的值**。在实际开发中，要尽量避免子类和父类使用相同的字段名，否则很容易引入一些不容易发现的bug。





**总结**

Java程序用于都分为**编译阶段**和**运行阶段**

- 实例变量：编译和运行都参考左边（静态绑定）
- 静态变量：编译和运行都参考左边（静态绑定）
- 静态方法、private方法和final方法：编译和运行都参考左边（静态绑定）
- 实例方法：编译参考左边，运行参考右边（动态绑定）

对上面的说法举个例子：

```java
class animal{
    public void eat(){
        System.out.println("动物吃饭");
    }
}

class dog extends animal{
    public void eat(){
        System.out.println("狗吃饭");
    }
    public void study(){
        System.out.println("狗学习");
    }
}

public class test{
    public static void main(String[] args){
        animal a = new dog();
        a.eat();
        a.study();
    }
}
```

- 注意
  - java程序永远分为编译阶段和运行阶段
  - 先分析编译阶段，再分析运行阶段，编译无法通过，根本是无法运行的
  - 编译阶段编译器检查a这个引用的数据类型为animal，由于animal.class字节码当中有eat()方法，所以编译通过了。这个过程为静态绑定，编译阶段绑定。只有静态绑定成功之后才有后续的运行
  - 在程序运行阶段，JVM堆内存当中真实创建的对象是dog对象，那么程序在运行阶段一定会调用dog对象的eat()方法，此时发生了动态绑定，运行阶段确定
  - 无论是dog类有没有重写eat()方法，运行阶段一定调用的是dog对象的eat方法(因为继承了)，因为底层真实对象就是dog对象
  - 父类型引用指向子类型对象这种机制导致程序在编译阶段绑定和运行阶段绑定两种不同的状态，这种机制称为多态语法机制
  
- 对于上面的a.eat()
  - 编译时：由于a的类型为animal，所以编译时先去检查animal类的方法区中有没有eat()方法，如果没有则报错。这就是所谓的**编译看左边**
  - 运行时：直接去dog类方法区中找到eat()方法；如果dog类中没有，再去父类中查找eat()方法。如果一直找到Object类都没有，则报错。这就是所谓的**运行看右边**
  
- 对于a.study()
  
  - 编译时：由于a的类型时animal，先去animal类的方法区中查找study()方法，结果animal类方法区中没有study()方法，所以就在编译时期就报错
  - 如果想执行study方法，该怎么办？--> **向下转型**
  
  ```java
  dog d = (dog)a;
  d.study();
  ```
  
  