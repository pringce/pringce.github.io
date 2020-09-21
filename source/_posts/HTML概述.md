---
title: HTML概述
date: 2020-06-17 20:16:27
tags:
- HTML
categories:
- HTML
mathjax: true
---

## 什么是HTML？

HTML：Hyper Text Markup Language（超文本标记语言）

由大量的标签组成，**每一个标签都有开始标签和结束标签**，一个标签里面可能有子标签，如下所示：

```html
<开始标签>
    <开始标签>
        <开始标签 属性名='属性值' 属性名='属性值'>
        </结束标签>    
    </结束标签>    
</结束标签>
```

超文本：流媒体、图片、声音、视频等



## 怎么开发HTML？

使用普通的文本编辑器就行，创建的文件扩展名是.html或者.htm

当然HTML也有专业的开发工具，例如DreamWeaver、HBuilder等

直接采用浏览器打开HTML就能运行



## HTML是谁制定的？

是由**W3C**（世界万维网联盟）制定的

W3C制定了HTML的规范，每个浏览器生产厂家都会遵守该规范

HTML程序员也会按照这个规范去写代码

W3C制定了很多规范：HTML/XML/http协议/https协议等



## 第一个HTML

```html
<!--
	这是HTML的注释
	加上<!doctype html>就表示HTML5语法，去掉就表示HTML4.0
	HTML不区分大小写，语法松散不严格
-->
<!doctype html>
<html>
	<!--头-->
	<head>
		<!--告诉浏览器采用哪种字符编码方式解析此文件
			注意：并不是设置当前文件的编码方式
		-->
		<meta charset="UTF-8">
		<!--网页的标题，显示在网页左上角-->
		<title>网页的标题</title>
	</head>
	<!--体-->
	<body>
		网页的主体内容
	</body>
</html>
```

这就是HTML代码的大体框架！！！