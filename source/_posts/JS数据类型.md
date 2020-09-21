---
title: JS数据类型
date: 2020-07-06 20:35:24
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

虽然JS的变量在声明的时候不需要指定数据类型，但是在赋值的时候，每一个数据还是有类型的

## JS的数据类型

包括原始类型和引用类型

- 原始类型

  - Undefined：只有一个值，就是undefined。当一个变量没有手动赋值，系统默认赋值undefined，或者也可以给一个变量手动赋值undefined

  - Number：包括**数字（整数和小数）、NaN（不是数字）、Infinity（无穷大）**。

    - 什么情况下结果是NaN？运算结果本来应该是一个数字，最后算完不是一个数字的时候，结果是NaN。例如

    ```javascript
    var a = 10;
    var b = "char";
    alert(a/b);//除号显然最后结果应该是数字，但是运算的过程中导致最后不是一个数字，结果是NaN
    alert(a+b);//这里会做字符串拼接，结果不是NaN，是"10char"
    ```

    - 当除数为0时，结果为Infinity
    - **isNaN()函数**，用法：isNaN(数据)，结果为true表示不是一个数字，false表示是一个数字。**isNaN=is not a Number**
    - **parseInt()函数**：可以将字符串自动转换成数字，并且取整数位
    - **parseFloat()函数**：可以将字符串自动转换成数字
    - **Math.ceil（)函数**：向上取整

  - String ：**可以使用单引号，也可以使用双引号**。其父类是Object

    - 怎么创建字符串对象？
      - 第一种：var s1 = "abc";**s1是String类型（小String）**，属于原始类型Stirng
      - 第二种：var s2 = new String("abc");**s2是Object类型（大String）**
    - 无论小String还是大String，它们的属性和函数都是通用的
    - **length属性**，可以获取字符串长度，如s1.length；结果为3
    - prototype属性，下面介绍Object的时候会说
    - String类型常用函数：
      - indexOf：获取指定字符串在当前字符串中第一次出现处的索引
      - lastIndexOf：获取指定字符串在当前字符串中最后一次出现处的索引
      - replace：替换（如果存在多个要替换，只会替换第一个；如果想全部替换，用正则表达式，即**加一个flags：g，代表全局匹配**，比如/\s+/g代表替换全部的空白符）。和Java的replace不太一样，Java中的是全部替换的，且Java还有repalceAll方法，而JS中没有该方法
      - substr：截取字符串，substr(2,4)：从2开始截取4个字符
      - substring：截取字符串，substring(2,4)，截取从2到4的字符串，不包括4
      - toLowerCse：转换小写
      - toUpperCase：转换大写
      - split：切分字符串

  - Boolean：只有两个值：true和false。在Boolean类型中有一个函数叫：Boolean(数据)，其作用是**将非Boolean类型转换为Boolean类型**。if语句的小括号里面的数据必须是Boolean类型，如果该数据是非Boolean类型，会自动调用Boolean()函数，将其转换成Boolean类型

    - Boolean()函数将哪些数据转换成true？**转换规律："有"就转换成true，"没有"就转换成false**。例如1就是有，0就是没有；""就是没有，"jack"就是有；null、NaN、undefined就是没有，Infinity就是有

  - Null：只有一个值，就是null。

  - Symbol(ES6及其之后版本才有这个类型)

- 引用类型

  - Object以及Object的子类
    - Object类型是所有类的超类，自定义的任何类，默认继承Object
    - 包括哪些属性？
      - prototype（常用）:给类动态的扩展属性和函数
      - constructor
    - 包括哪些函数？
      - toString()
      - valueOf()
      - toLocalString()
  - 在JS中定义的类默认继承Object，会继承Object类中所有的属性和函数，因此自定义的类中也有prototype属性
  - 定义类的语法：
  
  ```javascript
  //第一种方式
  function 类名(形参){
      
  }
  //第二种方式
  类名 = function(形参){
      
  }
  //创建对象的语法
  var o = new 类名(实参);//o是一个引用，指向堆内存的对象
  /*
  定义类和定义函数的方式一样
  重点看怎么使用：如果没有用new，那就是函数调用；如果使用了new，那就是创建对象
  */
  ```
  
  - **JS当中类的定义，同时又是一个构造函数的定义。在JS中类的定义和构造函数的定义是放在一起来完成的**
  
  ```HTML
  <script type="text/javascript">
      function user(a,b,c) {
          //必须要加this
          //如果不加this，就代表该变量是全局变量
          this.r = a;
          this.e = b;
          this.q = c;
          m = a;
          
          //定义类函数
          this.getA = function(){
              return this.a;
          }
  	}
      var o = new user(10,20,30);
      alert(o.r); //10
      
      //这里类中并没有定义m属性，为什么不报错？
      //因为这里是获取对象属性，对象是存在的，如果没有声明该属性，就会是undefined
      //如果alert(o.t),则还是undefined；但是如果alert(t)，就会报错，因为没有定义该变量
      //这里需要注意！！！
      alert(o.m);//undefined，因为m属性不属于user类，m是全局变量(没有用this)
      
      var pro = o.getA();//调用类中的函数
      alert(pro); //10
      
      //访问一个对象的属性，还可以使用如下语法:
      alert(o["r"]);
  </script>
  ```
  
  - 可以通过prototype这个属性给类动态扩展属性以及函数，如果扩展的新函数与类中的函数重名，那么会进行覆盖
  
  ```html
  <script type="text/javascript">
  	function User(name){
          this.name = name;
      }
      var pro = new user("lisi");
      
      //动态扩展函数
      User.prototype.getName = function(){
          return this.name;
      }
      var pname = pro.getName();
      alert(pname);//"lisi"
      
      //动态扩展属性
      User.prototype.no = 1314；
      alert(pro.no);//1314
      
      //由于String的父类是Object，所以也可以这样动态扩展
      String.prototype.add = function(){
          alert("new add");
      }
      "abc".add();//"new add"
  </script>
  ```
  
  

**JS中有一个运算符typeof，可以在程序的运行阶段动态的获取变量的数据类型**，语法格式为：typeof 变量名

typeof运算结果是以下6个字符串之一，注意字符串都是小写：

- "undefined"
- "number"
- "string"
- "boolean"
- "object"
- "function"

**在JS中比较字符串是否相等，采用==**

```javascript
var a;
typeof a; //"undefined"

var b = 10;
typeof b; //"number"

var c = "abc";
typeof c; //"string"

var d = null;
typeof d; //"object",但null属于Null类型，这里要注意

var f = false;
typeof f; //"boolean"

function say(){}
typeof say; //"function"
```



## null、NaN和undefined的区别

- 数据类型不一致
- null和undefined可以等同
- NaN与任何值都不相等，与自己也不相等

```javascript
alert(null==NaN) //false
alert(null==undefined) //true
alert(NaN==undefined) //false
alert(NaN==NaN) //false
```

- **在JS中有两个比较特殊的运算符：**
  - ==：等同运算符，只判断值是否相等
  - ===：全等运算符，即判断值是否相等，又判断数据类型是否相等