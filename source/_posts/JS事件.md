---
title: JS事件
date: 2020-07-07 09:42:08
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

## JS常用事件

- blur：失去焦点
- focus：获得焦点
- click：鼠标单击
- dblclick：鼠标双击
- keydown：键盘按下
- keyup：键盘弹起
- mousedown：鼠标按下
- mouseover：鼠标经过
- mousemove：鼠标移动
- mouseout：鼠标离开
- mouseup：鼠标弹起
- reset：表单重置
- submit：表单提交
- change：下拉列表选中项改变，或文本框内容改变
- select：文本被选定
- load：页面加载完毕，整个HTML页面中所有的元素全部即在完毕之后发生

**任何一个事件都会对应一个事件句柄，事件句柄是在事件前面添加on，事件句柄出现在一个标签的属性位置上，以属性的形式存在**



## 注册事件

- **第一种方式**：直接在标签中使用事件句柄

```html
<script type="text/javascript">
	function sayHello(){
        alert("hello");
    }
</script>

<!--
对于当前程序来说，sayHello函数被称为回调函数（callback函数）
回调函数的特点：自己把这个函数代码写出来了，但是这个函数不是自己负责调用，由其它程序负责调用该函数
-->
<input type="button" value="click" onclick="sayhello()">
```

**回调函数**：一般写程序是你调用系统的API，如果把关系反过来，你写一个函数，让系统调用你的函数，那就是回调了，那个被系统调用的函数就是回调函数。

同理，如果你调用系统写好的函数，那么对于该系统而言，该函数就是回调函数了

- **第二种方式**：使用纯JS代码完成事件的注册
  - 第一步：先获取按钮对象
  - 第二步：给按钮对象onclick赋值

```html
<input type="button" value="click" id="mybtn" />
<script type="text/javascript">
    function doSome(){
        alert("do some");
    }
    //第一步：获取按钮对象，document就代表整个HTML页面
    var btnObj = document.getElementById("mybtn");
    //第二步：给按钮对象的onclick属性赋值，将回调函数doSome注册到click事件上
    btnObj.onclick = doSome;//注意：千万别加小括号
</script>
```

**注意**：script脚本只能放在button定义下面才可以执行成功。因为浏览器是从上往下执行的，如果放在button上面的话，浏览器找不到mybtn的按钮，所以就不会给它添加事件了，会出现bug

针对上述注意所产生的问题，当然可以把button标签写在JS代码前。但是还有**另外一种解决方式**：采用load事件！（这种方式最为常用）

```html
<script type="text/javascript">
    //页面加载的过程中，将a函数注册给了load事件
    //页面加载完毕之后，load事件发生了，此时执行回调函数a
    //回调函数a执行的过程中，将b函数注册给了mybtn的click事件
    //当mybtn的节点发生了click事件十周，b函数被调用并执行
    window.onload = function(){//回调函数a
        document.getElementById("mybtn").onclick = function(){//回调函数b
            alert("do some");
        }
    }
</script>
<input type="button" value="click" id="mybtn" />
```



也可以直接使用匿名函数注册事件

```html
<input type="button" value="click" id="mybtn" />
<script type="text/javascript">
    var btnObj = document.getElementById("mybtn");
    //匿名函数的方式
    btnObj.onclick = function(){
        alert("test");
    }
</script>
```



## 捕捉回车键

回车键的键值是13

ESC键的键值是27

```html
<script type="text/javascript">
    window.onload = function(){
        var Elt = document.getElementById("username").onkeydown = function(event){
            //浏览器执行事件，调用回调函数时，会传过来一个事件对象，这里用event接收该事件对象
            //对于键盘事件对象来说，都有keyCode属性来获取键值
            //获取键值
            if(event.keyCode===13)
                alert("正在登录")；
        }
    }
</script>
<input type="text" id="username" />
```

Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。

事件通常与函数结合使用，函数不会在事件发生前被执行！

当一个事件发生的时候，和当前这个对象发生的这个事件有关的一些详细信息（包括导致事件的元素、事件的类型、以及其它与特定事件相关的信息等。这个对象是在执行事件时，浏览器通过函数传递过来的。）都会被临时保存到一个指定的地方——event对象，供我们在需要的时候调用

**在 W3C 规范中，event 对象是随事件处理函数传入的**，Chrome、FireFox、Opera、Safari、IE9.0及其以上版本都支持这种方式；但是对于 IE8.0 及其以下版本，event 对象必须作为 window 对象的一个属性。