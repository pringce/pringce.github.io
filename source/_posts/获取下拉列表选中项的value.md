---
title: 获取下拉列表选中项的value
date: 2020-07-13 10:22:00
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

通常会有这样的**需求**：前端浏览器首先展示一个下拉列表，用户可以

选择对应的省份；选择之后，需要获取到该下拉列表选中项value；将

该value提交给服务器，服务器底层执行一条SQL语句，返回一个List

集合：List< City> cityList。cityList响应浏览器，浏览器再解析cityList

集合转换成一个新的下拉列表展示在前端，让用户可以进一步选择相

应的城市

那么如何获取选中项的value传递给服务器呢？

**有两种方式**：

- **第一种**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>复选框的全选和取消全选</title>
    </head>
    <body>
        <select onchange="alert(this.value)">
            <option value="">----请选择省份----</option>
            <option value="hb">河北省</option>
            <option value="hn">河南省</option>
            <option value="sd">山东省</option>
            <option value="sx">陕西省</option>
        </select>
    </body>
</html>
```

- **第二种**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>复选框的全选和取消全选</title>
    </head>
    <body>
        <script type="text/javascript">
            window.onload = function(){
                var province = document.getElementById("province");
                province.onchange = function(){
                    alert(province.value);
                }
            }
        </script>
        <select id="province">
            <option value="">----请选择省份----</option>
            <option value="hb">河北省</option>
            <option value="hn">河南省</option>
            <option value="sd">山东省</option>
            <option value="sx">陕西省</option>
        </select>
    </body>
</html>
```

