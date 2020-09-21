---
title: JS函数
date: 2020-07-06 16:31:16
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

JS中的函数等同于Java语言中的方法，函数也是一段可以被重复利用的代码片段

函数一般都是可以完成某个特定功能的

回顾Java中的方法：**[修饰符列表] 返回值类型 方法名（形式参数列表）{方法体}**



**JS函数定义**

语法格式如下：

```javascript
//第一种方式
function 函数名(形式参数列表){
    函数体
}

//第二种方式
函数名 = function(形式参数列表){
    函数体
}
```

JS中的函数不需要指定返回值类型，返回什么类型都行

举个例子：

```javascript
function sum(a,b){
    //a和b都是局部变量，形参(a和b都是变量名，不需要类型)
    //不用写成function(var a,var b)
    alert(a+b);
}
```

**函数必须手动调用才能执行！！**不调用是不会执行的



```html
<script type="text/javascript">
    function sum(a,b){
        alert(a+b);
    }
</script>

<!--这里调用了sum函数-->
<!--这里和直接写alert函数一样，只不过alert是在自带的函数，而sum是我们自定义的-->
<input type="button" onclick="sum(10,20);">
```



Java中的方法有重载机制

JS中的函数在调用的时候，参数的类型和个数都没有限制。但是不能重载，**JS中如果出现两个同名函数，后面的函数会覆盖前面的函数**

```html
<script>
	function test(username){
        alert("test");
    }
    function test(){
        alert("test test")
    }
    
    //函数调用
    test("lisi");//test test，调用的是第二个函数
</script>
```

**在JS中分辨两个函数就是通过函数名，其余都不用**



```html
<script>
	function sum(a,b){
        alert(a+b);
    }
    
    var ret1 = sum(1,2);
    alert(ret1);//3
    
    var ret2 = sum("jack");//jack赋值给a，b采用系统默认赋值undefined
    alert(ret2);//jackundefined
    
    var ret3 = sum();
    alert(ret3);//NaN,是一个具体存在的值，该值表示不是数字(Not a Number)
    
    var ret4 = sum(1,2,3);//3没有被赋值
    alert(ret4);//3
    
</script>
```

从上面可以看到，一个函数可以被反复使用，根据在调用时不同的实参，会出现不同的结果