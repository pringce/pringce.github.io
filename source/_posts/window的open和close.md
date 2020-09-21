---
title: window的open和close
date: 2020-07-14 10:25:44
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

- BOM编程中，window对象是顶级对象，代表浏览器窗口
- window有open和close方法，可以开启窗口和关闭窗口



**open方法**

```html
<body>
    <!--默认打开新窗口-->
    <input type="button" value="新窗口" onclick="window.open('https://www.baidu.com')"> 
    <input type="button" value="当前窗口" onclick="window.open('https://www.baidu.com','_self')"> 
    <input type="button" value="新窗口" onclick="window.open('https://www.baidu.com','_blank')"> 
    <input type="button" value="父窗口" onclick="window.open('https://www.baidu.com','_parent')"> 
    <input type="button" value="顶级窗口" onclick="window.open('https://www.baidu.com','_top')"> 
</body>
```



**close方法**

关闭当前窗口

```html
<input type="button" value="关闭" onclick="window.close()" />
```

