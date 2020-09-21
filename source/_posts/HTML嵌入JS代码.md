---
title: HTML嵌入JS代码
date: 2020-07-06 11:03:22
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

共有**三种方式**在HTML中嵌入JS代码



## 第一种

要实现的功能：用户点击按钮，弹出消息框

JS是一门事件驱动型的编程语言，依靠事件去驱动，然后执行对应的程序。在JS中有很多**事件**，其中有一个事件叫做：鼠标单击（click）。并且任何时间都会对应一个**事件句柄**（事件和事件句柄的区别是：事件句柄是在事件单词的前面添加一个on）。click事件对应的事件句柄是onclick。**事件句柄是以HTML标签属性的方式存在**

**onclick="JS代码"的执行原理是什么？**页面打开的时候，js代码并不会执行，只是把这段js代码注册到按钮的click事件上了，等这个按钮发生click事件之后，注册在onclick后面的js代码会被浏览器自动调用

JS中的字符串可以使用双引号，也可以使用单引号

JS中的一条语句结束之后，可以使用分号，也可以不使用

```html
<input type="button" onclikc="alert('hello')" value="Click me!">
```

其中alert('hello')就是注册到click事件上的JS代码

也可以写多个alert，点击按钮之后会依次弹出，即第一个关闭之后就会弹出第二个，依次进行



## 第二种

脚本块方式

暴露在脚本块中的程序，在页面打开的时候执行

并且遵循自上而下的顺序依次执行（这个代码的执行不需要事件）

JavaScript的脚本块在一个页面中可以出现多次，没有要求

JavaScript的脚本块出现位置也没有要求，随意（任意位置都可以），但不同脚本块遵循自上而下的执行原则

```html
<script type="text/javascript">
	alert("hello");
</script>
```

**alert函数会阻塞整个HTML页面的加载**，只有把弹窗点击之后，才会继续加载



## 第三种

引入外部独立的JS文件，**推荐使用**

外部js文件（放在和html文件同级目录中的js文件夹中），文件名为1.js：

```javascript
//1.js
alert("hello");
```

html文件：

```html
<!doctype html>
<html>
    <head>
        <title>第三种方式</title>
    </head>
    <body>
        <script type="text/javascript" src=“js/1.js></script>
    </body>
</html>
```

在需要的位置引入js脚本文件

引入外部独立的js文件的时候，js文件中的代码会遵循自上而下的顺序依次逐行执行



同一个js文件可以被引入多次，但实际开发中这种需求很少



**当引入外部js文件时，不能在该< script>标签中写js代码了**

```html
<script type="text/javascript" src=“js/1.js>
	alert("test");
</script>
```

这里的alert("test")是不会被执行的！！



如果引入了外部js文件，在html文件中依旧可以用第二种方式嵌入js代码，但是遵循自上而下原则

```html
<script type="text/javascript" src=“js/1.js>
	alert("test");
</script>
<script type="text/javascript">
	alert("jack");
</script>
```

