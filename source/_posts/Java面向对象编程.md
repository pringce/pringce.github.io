---
title: Java面向对象编程
date: 2020-03-13 10:16:15
tags:
- Java
categories:
- Java
mathjax: true
---

# 什么是OOP？

- 面向对象编程：Object-Oriented Programming
- 对现实世界建立计算机模型的一种编程方法



面向过程和面向对象的区别？

- 面向过程：主要关注点是实现的具体过程，因果关系
  - 优点：对于业务逻辑比较简单的程序，可以达到快速开发，前期投入成本较低
  - 缺点：采用面向过程的方式很难解决非常复杂的业务逻辑，另外面向过程的方式导致软件元素之间的耦合度非常高。只要其中一环出问题，整个系统受到影响。导致最终的软件扩展力低。另外，由于没有独立体的概念，所以无法达到组件复用
- 面向对象：主要关注对象（独立体）能完成那些功能
  - 优点：耦合度低，扩展力强。更容易解决现实世界中更复杂的业务逻辑，组件复用性强
  - 缺点：前期投入成本较高，需要进行独立体的抽取，存在大量的系统分析与设计



面向对象的三大特性：**封装、继承、多态**。



采用面向对象的方式开发一个软件，生命周期当中存在：

- 面向对象分析OOA
- 面向对象设计OOD
- 面向对象编程OOP



## 类和对象的概念

​		**类**在现实世界中是不存在，是一个模板，是一个概念，是人类大脑思考抽象的结果。类代表了一类事物。在现实世界中，对象A和对象B之间具有共同的特征，进行抽象总结出一个模板，这个模板被称为类

​		**对象**是现实世界实际存在的个体



在Java程序中**通过类创建对象**

- 类 ——>  (**实例化**) ——> 对象（**对象又被称为实例**）
- 对象 ——>（**抽象**）——> 类
- 类描述的是对象的共同特征



一个类主要描述什么信息？

- 状态+动作

  - 状态信息：名字、身高、性别等
  - 动作信息：吃、唱歌、学习等

- 状态 ——> 一个类的属性

- 动作 ——> 一个类的方法

- 类{

  ​	属性 // 描述对象的状态信息

  ​	方法 // 描述对象的动作信息

  }

| 现实世界 | 计算机模型 |          Java代码          |
| :------: | :--------: | :------------------------: |
|    人    |  类/class  |       class Person{}       |
|   小明   | 实例/ming  | Person ming = new Preson() |
|   小红   | 实例/hong  | Person hong = new Person() |

| 现实世界     | 计算机模型 | Java代码                |
| ------------ | ---------- | ----------------------- |
| 书           | 类/class   | class Book{}            |
| Java核心技术 | 实例/book1 | Book book1 = new Book() |
| Java编程思想 | 实例/book2 | Book book2 = new Book() |



## **类/实例（class/instance）**

class是对象模板

- class定义了如何创建实例
- class名字就是数据类型

instance是对象实例

- instance是根据class创建的实例
- 可以创建多个instance
- 各个instance类型相同，但各自属性可能不同



## **定义class**

- 语法结构：

  [修饰符列表] class 类名{

  }

- 一个class可以包含多个field（字段），field用来描述一个class的特征

- class实现了**数据封装**

```java
public class Person{
    public String name;
    public int age;
}

public class Book{
    public String name;
    public String author;
    public String isbn;
}
```



## 创建实例

- **new操作符**可以创建一个实例，new运算符的作用是创建对象，在JVM堆内存当中开辟新的内存空间
- 定义一个引用类型变量来指向实例
- 通过变量来操作实例
- 通过**变量.字段**来访问实例字段

```java
public class Person{
    public String name;
    public int age;
}

Person ming = new Person();
ming.name = "小明";
ming.age = 12;
```



## 总结

- class和instance是"模板"和"实例"的关系
- class是数据类型，instace是数据
- class定义了field，每个instance都会拥有各自的field
- 变量指向instance，并通过**变量.字段**来访问实例字段
- 指向instance的变量都是引用变量



# 数据封装

- 一个class可以包含多个field
- 直接将field用public暴露给外部可能破坏了封装
- 用private修饰field可以拒绝外部访问
- 当field被设置为private时，可定义public方法间接修改field

```java
public class Person{
    private String name;
    private int age;
    
    public void setName(String name){
        this.name = name;
    }
}

Person ming = new Person();
ming.name = "小明"; //编译错误
ming.setName("小明"); //正确
```



## 方法

- 外部代码不可访问private字段
- 外部代码只能通过调用public方法间接设置和获取private字段
- public方法封装了数据访问
- 方法体中不能再定义方法
- 通过方法访问实例字段更安全
- 通过**变量.方法名()**来调用实例方法

### 定义方法

- 修饰符列表
- 返回值类型
  - 返回值类型可以是java中任意一种类型，包括基本数据类型和所有的引用数据类型
- 方法名称：合法的标识符即可
- 方法参数列表
  - 形参是局部变量
  - 形参中起决定作用的是形参的数据类型，形参的名字就是局部变量的名字
  - 方法在调用的时候，实际给这个方法传递的真实数据被称为实参
  - 形参列表和实参列表必须满足：
    - 数量和类型对应相同
- 方法返回值通过return语句实现，如果没有返回值（void）可以省略return。**只要带有return关键字的语句执行，return语句所在的方法结束。**
- **当一个方法有返回值时，在调用该方法时可以选择接受也可以选择不接收返回值。但是大部分条件下都是选择接收**

```java
public class Person{
    private String name;
    private int age;
    
    public void setName(String name){
        this.name = name;
    }
    
    public void setAge(int age){
        this.age = age;
    }
    
    public String getName(){
        return this.name;
    }
}
```

- 方法内部可以使用隐式变量this，**this指向当前实例**，this.field可以表示当前实例的字段
- 在不引起歧义的情况下，可省略this

```java
public String getName(){
        return name; //this.name
    }
```

- 局部变量名优先。当字段名和局部变量名重名时，编译器优先查找局部变量名

```java
public void setName(String name){
        this.name = name;
    }
```

### 调用方法

- 实例变量.方法名(参数)
- 可以忽略方法返回值

```java
Person ming = new Person();
ming.setName("小明"); // 没有返回值
String s = ming.getName(); //返回值为String
```

- 类内部调用方法

```java
public class hello{
    public static void main(String[] args){
        // 完整调用格式
        hello.qq();
        // 省略调用格式
        qq();
        
        // 编译正确
        A.a();
        // a(); 编译错误
        
        // 注意同一个类中调用方法可以省略类名，不同的类中调用方法不能省略类名
    }
    public static void qq(){
        System.out.ptintln("xixi");
    }
}

class A{
    public static void a(){
        System.out.ptintln("haha");
    }
}
```



### 方法参数传递

- 方法参数用于接收传递给方法的变量值
- 方法参数可为基本类型参数或引用类型参数。**可简单理解为基本类型是值传递，引用类型是址传递（但是无论是基本类西行还是引用类型，都是传递的是变量中保存的值，只不过有的是字面值，有的是Java对象在堆内存中的地址，所以才会有值传递和址传递的区别，但本质上都是一样的）**。见[方法调用时的参数传递问题](https://pringce.github.io/2020/04/04/方法调用时的参数传递问题/#more)

### private方法

- 外部代码不可访问private方法
- 内部代码可以调用自己的private方法



## 构造方法

前面可以看到，初始化实例需要三行代码

```java
Person ming = new Person(); // 创建对象实例
// 初始化对象实例
ming.setName("小明");
ming.setAge(12);
```

能否在创建对象实例时就把内部字段全部初始化为合适的值？如下所示

```java
Person ming = new Person("小明",12);
```

答案当然是肯定的！需要引入**构造方法（又被称为构造函数/ 构造器，constructor）**。构造方法用于创建对象

- 构造方法的作用

  - 创建对象
  - 创建对象的同时，初始化**实例变量**的内存空间
    - 成员变量之实例变量，属于对象级别的变量，这种变量必须先有对象才能有实例变量
    - 实例变量没有手动赋值的时候，系统默认赋值，那么这个系统默认赋值是在什么时候完成的？是在类加载的时候么？
      - 不是！因为类加载的时候只加载了代码片段，还没来得及创建对象，所以此时实例变量并没有初始化。
      - 实际上，实例变量的内存空间是在构造方法执行过程中完成开辟、初始化的。系统在默认赋值的时候，也是在构造方法执行过程当中完成的赋值
  
- **构造方法语法结构**

  - [修饰符列表] 构造方法名（形参列表）{

    ​		构造方法体；

    }

- **普通方法语法结构**

  - [修饰符列表] 返回值类型 构造方法名（形参列表）{

    ​		方法体；

    }

- 构造方法可以在创建对象实例时初始化对象实例

- **构造方法名就是类名，必须保持一致**

- 构造方法的参数没有限制

- **构造方法没有返回值(类型也没有void)** ，只要写上返回值类型就是普通方法了（不写返回值类型不代表没有返回值）

- **必须用new操作符调用构造方法**

  - 调用方法：new 构造方法名(实参列表)

- 构造方法调用后有返回值么？

  - 每一个构造方法实际上执行结束后都有返回值，但是不需要写，构造方法结束的时候java程序自动返回值，返回的是对象在堆内存中的地址。并且返回值类型是构造方法所在类的类型。由于构造方法的返回值类型就是类本身，所以返回值类型不需要编写

```java
public class Person{
    private String name;
    private int age;
   
    // 构造方法
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}
```

**如果一个类没有定义构造方法，编译器会自动生成一个默认构造方法，这个构造方法被称为缺省构造器：**

- 无参数
- 无执行语句

```java
public class Person{
    private String name;
    private int age;
    public Person(){
    }
}
```

**如果自定义了构造方法，编译器就不再自动创建默认构造方法。建议开发中手动的为当前类提供无参数构造方法，因为无参数构造方法太常用了**。所以构造方法支持重载机制，在一个类中编写多个构造方法，这多个构造方法显然已经构成方法重载机制

```java
public class Person{
    private String name;
    private int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
        // 如果自己写的构造函数中没有对所有的实例变量赋值，那么那些没有被赋值的实例变量会被系统赋默认值
    }
}

Person ming = new Person(); // 编译错误
// 因为此时没有默认构造方法了
```



可定义多个构造方法，编译器通过构造方法的**参数数量、位置和类型**区分

```java
public class Person{
    private String name;
    private int age;
    
    // 构造代码: new person("xiaoming",12);
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
    
    // 构造代码: new person("xiaoming");
    public Person(String name){
        this.name = name;
        this.age = 18;
    }
    
    // 构造代码: new person();
    public Person(){
    }
}
```



一个构造方法可以调法用其他构造方法，便于代码复用。调用其他构造方法的语法是**this(...)**

```java
public class Person{
    private String name;
    private int age;
    
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
    
    public Person(String name){
        this(name,18);
    }
    
    public Person(){
        this("unnamed");
    }
}
```



## 方法重载

### 什么时候考虑方法重载？

同一个类中功能相似的时候，尽可能让方法名相同

### 定义

方法重载（Overload）是指：

- **在同一个类中**

- 多个方法的方法名相同

- 但各自参数不同
  - 参数个数不同
  
  ```java
public static void m1(){}
  public static void m1(int a){}
  ```
  
  - 参数类型不同
  
  ```java
  public static void m2(int a){}
  public static void m2(double a){}
  ```
  
  - 参数位置不同
  
  ```java
  public static void m3(int a,double b){}
  public static void m3(double a,int b){}
  ```
  
- 方法重载**只和方法名+参数列表有关**，与返回值类型无关，与修饰符列表无关

  ```java 
  public static int q(){}
  public static void q(){} // 编译报错，因为就是同一个方法，没有构成重载
  ```

  

### 方法重载的目的

- 相同功能的方法使用统一名字
- 便于调用



## 方法递归

### 什么是递归

方法自身调用自身

```java
public static void doSome(){
    System.out.println("begin");
    doSome();
}
```

递归很耗费栈内存，递归算法可以不用的时候尽量别用。容易发生爆栈错误：java.lang.StackOverflowError。因此递归必须有结束条件，否则肯定会发生爆栈



## 继承

关于继承：

- 面向对象三大特征（封装、继承和多态）之一

- 继承基本作用是代码复用。但是继承最重要的作用是，有了继承才有了以后方法的覆盖的多态机制

- 继承语法格式：

  [修饰符列表] class 类名 extends 父类名{

  ​		类体;

  }

- java语言当中的继承只支持**单继承**，一个类不能同时继承多个类，C++支持多继承

- B类继承A类，其中

  - A类称为：父类、基类、超类、superclass
  - B类称为：子类、派生类、subclass

- 虽然java语言当中只支持单继承，但是一个类也可以间接继承其他类。例如

  - C extends B、B extends A、A extends T
  - C直接继承B，间接继承A、T

- java语言中假设一个类没有显示的继承任何类，该类默认继承javaSE库当中提供的java.lang.Object类。java语言当中任何一个类中都有Object类的特征

```java 
public class Person /* extends Object */ {
    private String name;
    private int age;
    public void run() {...}
}

public class Student extends Person{
    private int score;
    public void setScore(int score) {...}
    public int getScore() {...}
}
```

- Student可以从Person继承
- 继承使用关键字**extends**
- Student获得了Person所有的功能
- Student只需要编写新增的功能



### 继承树

{% asset_img 微信图片_20200314144110.png %}

Object是Java提供的所有类的根类，如果没有写extends，则默认继承自Object类。

- Java只允许class继承自一个类
- 一个类有且仅有一个父类（Object除外）

### protected

- Person类定义的private字段无法被子类访问
- 用protected修饰的字段可以被子类访问

```java
public class Person /* extends Object */ {
    protected String name;
    private int age;
    public void run() {...}
}

public class Student extends Person{
    public String hello(){
        return "hello"+this.name; // 编译通过
    }
}
```

### super

继承关系中的构造方法

```java
public class Person{
    public Person() {
        System.out.println("person");
    }
}

public class Student extends Person{
    public Student(){
        super();
        System.out.println("student");
    }
}
```

- super关键字表示父类(超类)
- 子类的构造方法的第一行语句必须调用父类的构造方法，调用方式为**super()**
- 没有super()时编译器会自动生成super()
- **如果父类没有默认构造方法，子类就必须显式调用super()**

```java
public class Person{
    public Person(String name) {
        System.out.println("person");
    }
}

public class Student extends Person{
    public Student(String name){
        //super(); // 这种构造会报错
        super(name); //这样才正确
        System.out.println("student");
    }
}
```



### 向上转型（upcasting）

子类型 --> 父类型，又称为自动类型转换

语法规则：<父类型> <引用变量名> = new <子类型>();**即父类引用指向子类对象**

```java 
Person p = new Student();
```

- 此时通过父类引用变量调用的方法是子类覆盖或继承父类的方法，不是父类的方法。
- 此时通过父类引用变量无法调用子类特有的方法



**向上转型虽然使代码变得简洁，体现了JAVA的抽象编程思想，但是也出现了上面提到的子类无法调用其独有的方法，这要怎么解决呢？所以就有了与之对应的向下转型，弥补了向上转型所带来的缺陷**。



### 向下转型（downcasting）

父类型 --> 子类型，又称为强制类型转换，需要加强制类型转换符

**什么时候要用向下转型?**

当调用的方法或属性是子类型中特有的，在父类型中不存在，必须进行向下转型

- 向下转型把抽象的类型变成一个具体的子类型

```java 
Person p = new Student();
Student s = (Student)p; // 向下转型
```

- 向下转型很可能报错：**java.lang.ClassCastException**

  - 举例

  ```java
  //animal是父类，cat和bird继承与animal类
  animal a = new bird();
  cat c = (cat)a;
  ```

  - 上面的程序编译是没有问题的，因为编译器检查到a的类型是animal，animal和cat存在继承关系，并且animal是父类，cat是子类，父类转换成子类叫做向下转型，语法正确

  - 程序虽然编译通过了，但是程序运行阶段会出现异常，因为JVM堆内存中真实存在的对象是bird类型，bird对象无法转换成cat对象，因为两种类型之间不存在任何继承关系，此时出现了著名异常

    - java.lang.ClassCastException
    - 类型转换异常，这种异常总是在向下转型的时候会发生

  - 如何避免类型转换异常？

    - 以上异常只有在向下转型会发生，也就是说向下转型存在安全隐患（编译通过，运行出错）。向上转型只要编译通过，运行一定不会出错

    - 使用instance of运算符可避免出现以上异常

      - 语法格式

      引用 instance of 数据类型名（类名）

      例：a instance of animal

      - 执行结果是boolean类型，true或false
      - true表示a这个引用指向的对象是一个animal类型
      - false表示a这个引用指向的对象不是一个animal类型

    - 所以在向下转型前，先判断一下引用指向的对象是否是你要转换的类型。例如

    ```java
    if(a instance of cat){
        cat c = (cat)a;
    }else if(a instance of bird){
        bird b = (bird)a;
    }
    ```
    - java规范中要求：在进行向下转型前，建议使用instance of进行判断，避免出现java.lang.ClassCastException异常



### 注意

无论是向上转型还是向下转型，**两个类之间必须有继承关系**，否则编译就将报错。



## 多态

### 多态的优点

提高了代码的扩展性，前期定义的代码可以使用后期的内容

### 多态的弊端

前期定义的内容不能使用（调用）后期子类的特有方法（就是多态调用的只能是父类）。但如果是继承子类覆盖了父类方法，多态调用的仍是子类的方法！

### 多态的类型

分为以下两种：

- 编译时多态：指的是 方法重载。**编译时多态是在编译时确定调用处选择那个重载方法，所以也叫 静态多态，算不上真正的多态**。所以，一般说的多态都是运行时的多态。
- 运行时多态：由于方法重写，所以想要确定引用变量所调用的方法的入口，必须根据运行时的引用变量所指向的实例对象来确定。从而使得同一个引用变量调用同一个方法，但不同的实例对象表现出不同的行为。



### 多态的前提条件

- 子类继承父类
- 子类**覆盖**父类的方法
- 父类引用指向子类对象



**多态性的实现**： 依靠动态绑定

**绑定**： 将一个方法调用与方法主体关联起来。 **前期绑定**： 在程序执行前绑定，由编译器和链接程序完成，C语言的函数调用便是前期绑定。 **动态绑定**： 也称 **后期绑定**。在运行时，根据具体的对象类型进行方法调用绑定。除了static方法、final方法（private方法也是final方法）和构造方法，其他方法都是动态绑定；



### 方法重载、重写与隐藏

#### **重载（Overload）**

方法重载就是在**同一个类**中，多个方法名称相同但是**参数类型或者参数个数不同或者参数顺序不同**的方法，**与返回值类型和修饰符无关**

 

#### **重写（Override）**

又被称为**方法覆盖**

**什么时候使用方法重写？**

- 当父类中的方法已经无法满足子类的业务需求，子类有必要将父类中继承过来的方法进行重新编写，这个重新编写的过程称为方法重写/方法覆盖

子类继承父类时，子类的**方法名称、参数类型、参数个数**与父类**完全相同**，则认为子类重写了父类的方法。

**方法重写规则：**

- **方法重写发生在具有继承关系的父子类之间**
- 参数列表和原方法完全相同
- 方法名相同
- **返回值类型和原方法相同或者为父类返回值类型的子类型**
- 不能比原方法限制更严格的访问级别(举例：父类方法为public，那么子类不能为protected、private)
- 不能抛出新的异常或比原方法更广泛的异常（父类抛出IOException，重写方法不能抛出Exception只能抛出IOException或者IOException子类异常）。如果父类方法没有抛出异常，那么子类方法也不能抛出任何异常



**注意**

- 父类方法被定义为final时，可以被继承，但不能被重写，也不能被隐藏。final方法是防止子类覆写修改，子类继承直接使用是可以的
- 父类方法被定义为static时，不能被重写，但是可以声明一个相同的方法（参考**隐藏**）
- 构造方法不能被继承，所以不能重写，也不能隐藏
- private方法虽然可以被继承，但是无法访问，所以如果定义一个一模一样的private方法，它们其实是两个相互独立的方法，而不是重写，也不是隐藏。（这里压根就没有重写或隐藏的概念，因为在子类中压根就对父类的private方法不可见，虽然可以继承）
- **覆盖只针对部分实例方法（除构造方法、final方法和private方法），不针对属性**





**方法重写的条件**

- 重写的方法是子类从父类继承下来的**实例方法**（就是相对于静态方法而言的，用对象访问的那些方法），不能是静态方法
- 子类重写后的方法的 返回类型 必须是 原父类方法的返回类型的**可替换类型**
- 子类重写后的方法的访问权限 不能比 原父类方法的访问权限低；
- 子类重写后的方不能比父类方法抛出更多的异常；
- **当重写泛型方法时，先进行类型擦除。再按照上面的4个小点，重写类型擦除后的方法;**



**可替换类型补充：**

- 对于返回类型是基本类型、void，重写方法的返回类型必须是一样；
- 对于返回类型是引用类型，返回类型可替换成该类型的 子类型;

```java
class ParentClass{//父类
    public int count() {//
        return 0;
    }
    
    Object method() {
        return "aa";
    }
}

class ChildClass extends ParentClass{//子类
    public int count() {//重写count()方法，由于返回类型是基本类型，不能变，必须是一致
        return 0;
    }
    
    public String method() {//重写method()：访问权限增大，返回类型是Object的子类String
        return "aa";
    }
}

```



#### 隐藏

隐藏是针对于**父类的成员变量和静态方法**而言的。子类中声明了和父类相同的变量名或静态方法(方法名相同、参数列表相同、返回类型相同)则实现了对父类成员变量和静态方法的隐藏。从父类继承下来的成员中，除了部分方法是可以重写外，其余成员都是隐藏，如变量、内部类、静态方法等。

**注意：final方法既不能被重写，也不能被隐藏；private方法没有重写和隐藏的概念**



**JAVA中方法和变量在继承时的覆盖和隐藏规则**

- 父类的实例变量和静态变量能被子类的同名变量**隐藏**
- 父类的静态方法被子类的同名静态方法**隐藏**
- 父类的实例方法被子类的同名实例方法**覆盖**



**换言之，多态是基于重写实现的，针对实例方法。变量、static方法、final方法和private方法不能重写。其中final方法和private方法也不能隐藏。**

### 多态举例

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
        f.eat();
        //f.play(); 编译错误
        System.out.println("年龄："+f.age );
        
    }

}
```

**结论：**当满足Java多态的三个条件时，可以发现f.eat()调用的实际上是子类的eat，但f.age调用的还是父类的age，而**f.play()则不会通过编译**。(**多态实现机制可看另一篇文章：从JVM角度看Java多态**)