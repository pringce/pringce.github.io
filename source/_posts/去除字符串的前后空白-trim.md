---
title: 去除字符串的前后空白-trim
date: 2020-07-12 11:09:12
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

在做表单验证的时候，通常需要对字符串进行去除前后空白的操作，String类中有trim()方法可以完成上述操作，例如：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>RegExp</title>
    </head>
    <body>
        <script type="text/javascript">
            window.onload = function(){
                document.getElementById("btn").onclick = function(){
                    var user = document.getElementById("user").value;
                    var newUser = user.trim();
                    alert("------->"+newUser+"<-------");
                }
            }
        </script>

        <input type="button" value="click" id="btn" />
        <input type="text" id="user" />
    </body>
</html>
```

**注意**：上述的代码在Chorme、FireFox可以正常运行，但是对于低版本的IE（比如IE8）却不能正常运行。解决方案如下：

**即对String类扩展trim方法，这样就会覆盖String类自带的trim方法**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>RegExp</title>
    </head>
    <body>
        <script type="text/javascript">
            //低版本的IE浏览器不支持字符串的trim函数，怎么办？
            //可以自己对String类扩展一个全新的trim()函数
            String.prototype.trim = function() {
                //在当前的方法中，this代表的是当前字符串
                //先去除前空白，再去除后空白
                return this.replace(/^\s+/,"").replace(/\s+$/,"");
            };
            window.onload = function(){
                document.getElementById("btn").onclick = function(){
                    var user = document.getElementById("user").value;
                    var newUser = user.trim();
                    alert("------->"+newUser+"<-------");
                }
            }
        </script>

        <input type="button" value="click" id="btn" />
        <input type="text" id="user" />
    </body>
</html>
```

