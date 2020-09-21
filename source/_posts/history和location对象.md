---
title: history和location对象
date: 2020-07-14 11:37:18
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

## history对象

通常用来前进和后退页面

**hello.html**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>顶级窗口</title>
    </head>
    <body>
        <a href="login.html">login页面</a>
        <!--前进-->
        <input type="button" value="前进" onclick="window.history.go(1)">
    </body>
</html>
```

**login.html**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>history</title>
    </head>
    <body>
        login page!
        <!--以下两种方式均可以回退-->
        <input type="button" value="后退" onclick="window.history.back()">
        <input type="button" value="后退" onclick="window.history.go(-1)">
    </body>
</html>
```

- 在hello页面中点击超链接会跳转至login页面，然后点击后退按钮会回退至hello页面
- 刚打开hello页面时，点击前进按钮不会有任何反应，因为没有history；当点击超链接跳转之后再回退至hello页面，此时点击前进按钮，就会前进至login页面



## location对象

设置浏览器地址栏上的URL

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>顶级窗口</title>
    </head>
    <body>
        <script type="text/javascript">
            function goBaidu(){
                //以下两种方式均可
                window.location.href = "https://www.baidu.com";
                //document.location.href = "https://www.baidu.com";
            }
            }
        </script>
        <input type="button" value="百度" onclick="goBaidu()">
    </body>
</html>
```

此时点击"百度"按钮之后，就会跳转至百度页面



**总结**：有哪些方法可以通过浏览器往服务器发请求？

- 表单form的提交
- 超链接
- document.location
- window.location
- window.open("url")
- 直接在浏览器地址栏上输入url，然后回车

**注意**：以上所有的请求方式均可以携带数据给服务器，只有通过表单提交的数据才是动态的，其余就是直接将数据写进url，例如url?name1=pwd1&name2=pwd2...