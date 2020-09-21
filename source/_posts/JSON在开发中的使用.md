---
title: JSON在开发中的使用
date: 2020-08-04 20:31:30
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---



## 什么是JSON？

JavaScript Object Notation，简称JSON。是一种**数据交换格式**

JSON主要的作用是一种标准的数据交换格式。系统A和系统B交换数据的话，都是采用JSON

JSON是一种标准的轻量级的数据交换格式，特点是**体积小，易解析**

在实际的开发中，有两种数据交换格式使用最多，**分别是JSON和XML**。**XML体积较大，解析麻烦，但是语法严谨**（通常银行相关的系统之间进行数据交换时采用XML）



下面展示一下XML的格式：

```xml
<!--students.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<students>
	<student sno="110">
		<name>张三</name>
		<sex>男</sex>
	</student>
	<student sno="120">
		<name>李四</name>
		<sex>男</sex>
	</student>
	<student sno="130">
		<name>王五</name>
		<sex>男</sex>
	</student>
</students>
```

可以看到XML和HTML很相似，其实HTML和XML有一个父亲：SGML（标准通用的标记语言）

HTML主要做页面展示，所以语法松散，很随意

XML主要做数据存储和数据描述的，所以语法相当严格



再展示一下JSON的格式：

```HTML
<script type="text/javascript">
    //创建JSON对象
    var studentObj = {
        "sno" : "110",
        "sname" : "张三",
        "sex" : "男"
    };
    //访问JSON对象的属性
    alert(studentObj.sno+","+studentObj.sname+","+studentObj.sex)
    
    //JSON数组
    var students = [
        {"sno" : "110","sname" : "张三","sex" : "男"},
        {"sno" : "120","sname" : "李四","sex" : "男"},
        {"sno" : "130","sname" : "王五","sex" : "男"}
    ];
    //遍历
    for(var i = 0;i<studens.length;i++){
        var stuObj = students[i];
        alert(stuObj.sno+","+stuObj.sname+","+stuObj.sex)
    }
</script>
```

**JSON语法格式**：

```json
var Obj = {
    "属性名" : 属性值,
    "属性名" : 属性值,
    "属性名" : 属性值,
    "属性名" : 属性值
    ...
}
```



**复杂一些的JSON**

JSON嵌套

```json
var user = {
    "usercode" : 110,
    "username" : "张三",
    "sex" : true,
    "address" : {
        "city" : "北京",
        "street" : "大兴区"
    }
}

//访问居住城市
alert(user.address.city);
```



**eval函数**

将字符串当作一段JS代码解释并执行

```html
<script>
	window.eval("var i = 100;");
    alert("i="+i); //i=100
</script>
```

为什么要说这个呢？

因为java连接数据库，查询数据之后，将数据在java程序中拼接成JSON格式的字符串，将JSON格式的字符串响应到浏览器。也就是说，java响应到浏览器上的仅仅是一个JSON格式的字符串，还不是一个JSON对象。可以使用**eval函数**，将JSON格式的字符串转换成JSON对象

```html
<script>
    //这是从java发过来的json格式字符串
	var fromJava = "{\"name\":\"zhangsan\",\"pwd\":123}";
    //转换成json对象
    window.eval("var jsonObj"+fromJava);
</script>
```



**在JS中，[]和{}有什么区别？**

- []是数组
- {}是JSON
- java中的数组：int[] arr = {1,2,3,4,5};
- JS中的数组：var arr = [1,2,3,4,5];
- JSON：var jsonObj = {"name" : "张三", "age" : 25};



## 下面写一个JSON常用的场景

**设置table的tbody（即点击按钮之后，在表格中显示从后台传来的数据）**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>设置table的tbody</title>
    </head>
    <body>
        <script type="text/javascript">
            var data = {
                "total" : 4,
                "emps" : [
                    {"empno":1,"ename":"smith","sal":800},
                    {"empno":2,"ename":"jack","sal":900},
                    {"empno":3,"ename":"rose","sal":1200},
                    {"empno":4,"ename":"jenny","sal":700}
                ]
            };
            //把数据展示到table中
            window.onload = function(){
                var displayBtnElt = document.getElementById("displayBtn");
                displayBtnElt.onclick = function(){
                    var emps = data.emps;
                    var html = "";
                    for(var i=0;i<emps.length;i++){
                        var emp = emps[i];
                        html += "<tr>";
                        html += "<td>"+emp.empno+"</td>";
                        html += "<td>"+emp.ename+"</td>";
                        html += "<td>"+emp.sal+"</td>";
                        html += "</tr>";
                    }
                    document.getElementById("emptbody").innerHTML = html;
                    document.getElementById("count").innerHTML = data.total;
                }
            }

        </script>

        <input type="button" value="员工信息列表" id="displayBtn" />
        <h2>员工信息列表</h2>
        <hr>
        <table border="1px" width="50%">
            <tr>
                <th>员工编号</th>
                <th>员工名字</th>
                <th>员工薪资</th>
            </tr>
            <tbody id="emptbody"></tbody>
        </table>
        总共<span id="count">0</span>条数
    </body>
</html>
```

**点击按钮前的效果：**

{% asset_img 1.png %}



**点击按钮后的效果：**

{% asset_img 2.png %}

