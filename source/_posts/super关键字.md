---
title: super关键字
date: 2020-04-08 11:36:26
tags:
- Java
categories:
- Java
mathjax: true
---

super：

- super代表的是“当前对象（this）”的那部分“父类型特征”，所以在创建子类对象的时候并没有创建父类型对象，而是在子类对象中有一部分是父类型特征。
  - 比如我继承了我父亲的眼睛、皮肤，这些虽然继承于父亲，但这部分是在我身上的
- 和this对比
  - this只能出现在实例方法和构造方法，不能使用在静态方法。大部分情况下可以省略，**在区分局部变量和实例变量时不能省略**。有两种语法：1、this.表示调用本类变量或方法；2、this()表示通过当前构造方法调用**本类**其它构造方法，**只能出现在构造方法第一行**
  - super只能出现在实例方法和构造方法，不能使用在静态方法。大部分情况下可以省略，**如果父类和子类有同名变量，并且想在子类中访问父类变量，super.不能省略**。有两种语法：1、super.表示调用变量或方法；2、super()表示通过当前构造方法调用**父类**其它构造方法，**只能出现在构造方法第一行**，创建子类对象的时候先初始化父类型特征
  - 当一个构造方法的第一行既没有this()又没有super()，默认会有一个super()；表示通过当前子类的构造方法调用父类的无参数构造方法
  - this()和super()不能共存，因为他们都需要在构造函数的第一行
  - **super不是引用，不保存内存地址，也不指向任何对象，只是代表当前对象内部的那一块父类型特征**，所以super不能单独打印输出，但this可以单独打印输出

super关键字使用情况，主要有以下三种：

- **子类方法中调用被重写的父类的方法**

```java
class A {
	private String nameA="A";
	public void getName() {
		System.out.println("父类"+nameA);
	}
}
 
class B extends A{
	private String nameB="B";
	@Override
	public void getName() {
		System.out.println("子类"+nameB);
		super.getName();
	}
}

public class Test{
    public static void main(String[] args) {
		B f=new B();
		f.getName();
    }
}
```

运行结果为：

```java
子类B
父类A
```

结果分析：

在子类B中，我们重写了父类的getName方法，如果在重写的getName方法中我们去调用了父类的相同方法，必须要通过super关键字显示的指明出来。

如果不明确出来，按照子类优先的原则，相当于还是再调用重写的getName()方法，此时就形成了死循环，会一直输出”子类B“。执行后会报java.lang.StackOverflowError异常。



- **子类中访问被隐藏的父类成员变量**

```java
class A {
	 String nameA="A";
}
 
class B extends A{
	 String nameA="B";
	public void getName() {
		System.out.println("子类"+nameA);
		System.out.println("父类"+super.nameA);
	}
}

public class Test{
    public static void main(String[] args) {
		B b=new B();
		b.getName();
	}
}
/* 输出结果为：
*  子类B
*  父类A
*/
```

此时子类B中有一个和父类一样的字段（也可以说成父类字段被隐藏了），为了获得父类的这个字段我们就必须加上super，如果没有加，都是访问的子类B里面的那么字段。

我们通过**super是不能访问父类private修饰的变量和方法**



- **在子类构造方法中调用父类构造方法**
  - 编译器会自动在子类构造函数的第一句加上 super(); 来调用父类的无参构造器；此时可以省略不写。如果想写上的话必须在子类构造函数的第一句，可以通过super来调用父类其他重载的构造方法，只要相应的把参数传过去就好
  - **在java语言中不管new什么对象，最后object类的无参构造方法一定会执行，且一定是最先执行的**
  - 但是有时候会存在问题：
    - 如果一个类中没有写任何的构造方法，JVM会生成一个默认的无参构造方法。在继承关系中，由于在子类的构造方法中，第一条语句默认为调用父类的无参构造方法（即默认为super()，一般这句话省略了）。但是当在父类中定义了有参构造函数，没有定义无参构造函数时，IDE会强制要求我们定义一个相同参数类型的构造器。如果在子类构造函数中没有显示的调用父类中的有参构造函数会报错。所以此时必须要显示的调用有参构造函数

```java
class father{
    public father(String name){
        System.out.println(name);
    }
}

class son estends father{
    /* 报错，因为默认调用super()，但是父类中没有无参构造方法
    public son(){
 
    }
    */ 
    
   	// 下面代码编译正确
    public son(){
        super("我是子类传过去的");
    }
}
```

