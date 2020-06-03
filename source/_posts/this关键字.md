---
title: this关键字
date: 2020-04-06 10:07:55
tags:
- Java
categories:
- Java
mathjax: true
---

先看一段代码：

```java
// customer.java
public class customer{
    // 姓名
    String name; // 实例变量必须采用“引用.”的方法访问
    // 构造方法
    public customer(){
    }
    
    // 不带有static关键字的一个方法
    // 顾客的购物行为，每一个顾客购物的最终结果是不一样的
    // 所以购物这个行为是属于对象级别的行为
    // 由于每一个对象在执行购物这个动作的时候最终结果不同，所以购物这个动作必须有“对象”的参与
    // 没有static的方法是实例方法，“引用.”的方式调用
    // 当一个行为执行中是需要对象参与的，那么一定要定义为实例方法
    public void shopping(){
        System.out.println(this.name + "在购物");
    }
}

// customerTest.java
public class customerTest{
    public static void main(String[] args){
        // 创建customer对象
        customer c1 = new customer();
        c1.name = "zhang";
        c1.shopping(); // zhang在购物
        // 创建customer对象
        customer c2 = new customer();
        c2.name = "li";
        c2.shopping(); // li在购物
    }
}
```

上述代码内存图如下所示：

{% asset_img 1.png %}



关于Java语言当中的this关键字：

- this是一个关键字，翻译为这个
- this是一个引用，this是一个变量，this变量中保存了内存地址指向了自身，this存储在JVM堆内存Java对象内部
- this就是代表当前对象
- 创建100个Java对象，每一个对象都有this，也就是说有100个不同的this
- this可以出现在**实例方法**当中，**this指向当前正在执行这个动作的对象**
- **this在多数情况下可以省略不写（不造成歧义的情况下）**。
  - this在区分实例变量和局部变量的时候不能省略。下面这个例子如果省略了this就会造成歧义

```java
public class hello{
    int age;
    String name;
    public hello(int age,String name){
        this.age = age;
        this.name = name;
    }
    /*
    * 这种情况下可以省略
    * public hello(int a,String n){
    *     age = a;
    *     name = n;
    */
    }
}
```

- **this不能使用在静态方法当中（因为实例方法属于类，没有对象也可以访问，所以this用不了）**
- 如果静态方法里想访问实例变量怎么办？
  - 在方法里创建对象即可

```java
public static void doSome(){
    customer c = new customer();
    System.out.println(c.name);
}
```

- **构造方法、实例方法的第一个参数是this**！这是由编译器自动添加的
  - 实例方法中（非构造非静态）：this指代的是正在调用当前方法的对象
  - 构造方法中：this指代的是正在new的对象本身
- **个人小理解**：
  - 调用实例或构造方法肯定是堆中的对象来调用的，因为它们存有方法区中方法的入口地址。但是堆中可能存在同一个类的多个对象，但是方法区中的代码只有一份，如何指定调用的是哪一个的对象的方法，以使得产生不同的运行结果？比如给实例变量赋值（选择给哪个对象的实例变量赋值呢？）因为不同对象有差异性，但方法区里面的代码是唯一的，这个时候将每个对象内部的this作为参数传递给实例或构造方法，就可以解决上面的指定问题了。因为不同的对象是有不同的this的，this指向堆内存对象本身。
- 同一类中的一个构造方法可以调法用类中其他构造方法，便于代码复用。调用其他构造方法的语法是**this(实参)**，并且只能出现在第一行
- this既可以使用在实例方法中，也可以使用在类中其它构造方法中。但不能使用在静态方法中
- 静态方法既可以采用类名的方式访问，也可以采用引用的方式访问。但是即使采用引用的方式访问，实际上执行的时候和引用指向的对象无关。可以简单的认为在执行的时候还是把前面的引用换成了类，本质还是类名的方式访问

