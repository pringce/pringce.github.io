---
title: JS变量
date: 2020-07-06 15:55:34
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

JS的标识符命名规则和规范按照java执行：

**命名规则**：标识符必须是英文字母、数字、下划线和美元符号$的组合；且不能以数字开头；不能使用关键字

**标识符命名规范**（驼峰）：首字母小写，后面每个单词首字母大写



java语言是一种**强类型语言**，强类型怎么理解？

java语言存在编译阶段，假设有代码：int i；

那么在java中有一个特点是：java程序在编译阶段就已经确定了i变量的数据类型，该i变量的数据类型在编译阶段是int类型。那么这个变量直到最终内存释放，一直都是int类型，不可能变成其它类型



**JS声明变量**

```javascript
var 变量名;
```

**JS变量赋值**

```javascript
变量名 = 值;
```

JavaScript是一种**弱类型语言**，没有编译阶段，一个变量可以随意赋值，赋什么类型的值都行

变量的类型取决于等号右边，赋什么类型的值，该变量就是什么类型，可以动态的改变。但是java就不行，变量的类型取决于等号左边，在编译阶段就已经确定

变量如果没有手动赋值，系统默认赋值**undefined**（undefined在JS中是一个具体存在值）

变量必须声明之后才可以访问！否则会有语法错误



注意：多个变量声明应注意以下内容

```javascript
//a、b都是undefined，c是200
var a,b,c = 200;
```



**全局变量和局部变量**

- **全局变量**：在函数体之外声明的变量，生命周期是浏览器从打开到关闭，尽量少用，太占浏览器的内存。能使用局部变量尽量使用局部变量
- **局部变量**：在函数体内声明的变量，包括一个函数的形参。局部变量的生命周期是：函数开始执行时局部变量的内存空间开辟，函数执行结束之后，局部变量的内存空间释放

**注意**：全局变量和局部变量重名时，在函数体内访问该变量，局部变量会覆盖全局变量，即访问的是局部变量

**注意**：如果一个变量在声明时没有使用var关键字，不管在哪里声明，该变量都是全局变量

```javascript
function myfun(){
    name = "lisi";//name是全局变量
}
```



**JS创建数组**

```javascript
//创建数组长度为0的数组
var arr = [];
var array = [false,true,1,2,"abc"];
```

JS中的数组的数据类型随意，元素个数随意，可以自动扩容，这里和Java是有区别的

**遍历数组**

```javascript
for(var i=0;i<array.length;i++){
    alert(array[i]);
}
```



**运算符之void**

**需求**：既保留住超链接的样式，同时用户点击该超链接的时候执行一段JS代码，但页面还不能跳转

先尝试写一下

```html
<a href="https://www.baidu.com" onclick="alert(test)">百度</a>
```

上述代码执行之后，会有超链接的样式；点击该超链接之后会执行JS代码，产生弹窗；但是关闭该弹窗之后，还是会跳转

为了解决上述问题，使用void关键字

```html
<a href="javascript:void(0)" onclick="alert(test)">百度</a>
```

**void运算符的语法：void(表达式)。运算原理：执行表达式，但不返回任何结果**





**JS控制语句**

- if
- switch
- while
- do while
- for循环
- break、continue
- for...in语句（JS特有）

**用for...in遍历数组**

```javascript
for(var i in array){
    //这里的i是数组下标，不同于java的for...each
    alert(arr[i]);
}
```

- with语句（JS特有）

```javascript
function user(a,b,c) {
        this.r = a;
        this.e = b;
        this.q = c;
	}
    var o = new user(10,20,30);
    alert(o.r); //10

	//使用with语句
	with(o){
        alert(r);//10
    }
```

**前6个和java的控制语句一模一样**