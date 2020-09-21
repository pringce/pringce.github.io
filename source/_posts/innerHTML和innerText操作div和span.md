---
title: innerHTML和innerText操作div和span
date: 2020-07-11 10:24:45
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

innerText和innerHTML属性有什么区别？

**相同点：**

​	都是设置或获取元素内部的内容

**不同点：**

​	几乎所有的元素都有innerHTML属性，它是一个**字符串**，用来**设置或获取**位于对象起始和结束标签内的HTML(包含html标签)。当设置的时候，innerHTML会把后面的“字符串”**当作一段HTML代码解释并执行**。例如：

```html
<!--如果通过id获取到div标签对象divElt，那么divElt.innerHTML就是："<span>i love</span>"-->
<div id="div">
    <span>i love</span>
</div>
```

​	innerText可**获取或设置**指定元素标签内的文本值，从该元素标签的起始位置到终止位置的全部文本内容(不包含html标签)。当设置的时候，innerText即使后面是一段HTML代码，也只是将其当作普通的字符串来看待。例如：

```html
<!--如果通过id获取到div标签对象divElt，那么divElt.innerText就是："i love"-->
<div id="div">
    <span>i love</span>
</div>
```



## innerHTML属性

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>innerHTML和innerText</title>
        <style type="text/css">
            #div1{
                background-color: aquamarine;
                height : 300px;
                width : 300px;
                position : absolute;
                top : 100px;
                left : 100px;
            }
        </style>
    </head>
    <body>
        <script type="text/javascript">
            window.onload = function(){
                document.getElementById("btn").onclick = function(){
                    var divElt = document.getElementById("div1");
                    divElt.innerHTML = "I LOVE YOU";   
                    divElt.innerHTML = "<font color='red'>I LOVE YOU</font>";
                }
            }
        </script>

        <input type="button" value="click" id="btn" />
        <div id="div1"></div>
    </body>
</html>
```

效果如下：

{% asset_img 1.png %}



## innerText属性

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>innerHTML和innerText</title>
        <style type="text/css">
            #div1{
                background-color: aquamarine;
                height : 300px;
                width : 300px;
                position : absolute;
                top : 100px;
                left : 100px;
            }
        </style>
    </head>
    <body>
        <script type="text/javascript">
            window.onload = function(){
                document.getElementById("btn").onclick = function(){
                    var divElt = document.getElementById("div1");
                    divElt.innerText = "<font color='red'>I LOVE YOU</font>";
                }
            }
        </script>

        <input type="button" value="click" id="btn" />
        <div id="div1"></div>
    </body>
</html>
```

效果如下：

{% asset_img 2.png %}





上面是设置innerHTML和innerText。下面写一个获取innerHTML和innerText的例子

```html
<script type="text/javascript">
    window.onload = function(){
        document.getElementById("btn").onclick = function(){
            var divElt = document.getElementById("div");
            document.getElementById("user").value = divElt.innerText;
            alert(divElt.innerHTML);
        }
    }
</script>

<input type="button" value="click" id="btn" />
<input type="text" id="user" />
<div id="div">
    <span>i love</span>love
</div>
```

效果如下：

{% asset_img 3.png %}

可以看到，点击click按钮之后，文本框被写入了div标签内的全部文本内容（不包括html标签）；而弹出的窗口中则是div标签内的html语句（包括html标签）