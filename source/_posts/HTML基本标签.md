---
title: HTML基本标签
date: 2020-06-17 21:03:51
tags:
- HTML
categories:
- HTML
mathjax: true
---

## 段落标签

先看一下不加段落标签的样子：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
		这是第一段
		这是第二段
		这是第三段
	</body>
</html>
```

浏览器界面如下：

{% asset_img 1.png %}



然后我们加上段落标签，方法为：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
		<p>这是第一段</p>
		<p>这是第二段</p>
		<p>这是第三段</p>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 2.png %}



## 标题字

这是HTML预留的格式，和word中的标题字相同。有h1到h6这六种：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
		<h1>标题</h1>
		<h2>标题</h2>
		<h3>标题</h3>
		<h4>标题</h4>
		<h5>标题</h5>
		<h6>标题</h6>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 3.png %}



## 换行标记

< br >标签

这种标签只有一个，叫做**独目标记**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
		hello<br>world!
	</body>
</html>
```

浏览器界面如下：

{% asset_img 4.png %}



## 水平线

< hr >标签

也是一个独目标记

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
		hello<br>world!
		<hr>
		<!--颜色为红色，宽度占页面宽度的50%-->
        <!--color和width都是hr标签的属性-->
        <!--属性值可以加双引号、单引号甚至没引号，都可以！-->
		<hr color="red" width="50%">
	</body>
</html>
```

浏览器界面如下：

{% asset_img 5.png %}



## 预留格式

我们先看一下下面这个例子：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
	for(int i=0;i<10;i++){
		System.out.println(i);
	}
	</body>
</html>
```

这个for循环肯定不能按照我们写的格式在浏览器上展现出来，最后发现上面的三行for循环最后出现在一行上！

如何让它保持原样出现在浏览器上呢？**使用< pre >< /pre >标签**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
	<pre>
	for(int i=0;i<10;i++){
		System.out.println(i);
	}
	</pre>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 6.png %}



## 各种字体格式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
	<!--删除字-->
	<del>删除字</del>
	<!--插入字-->
	<ins>插入字</ins>
	<!--粗体字-->
	<b>粗体字</b>
	<!--斜体字-->
	<i>斜体字</i>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 7.png %}



## 右上角/右下角加字

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
	<!--右上角-->
	10<sup>2</sup>
	<!--右下角-->
	10<sub>2</sub>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 8.png %}



## 字体标签

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML基本标签</title>
	</head>
	<body>
	<!--红色，大小为50-->
	<font color="red" size="50">字体标签</font>
	</body>
</html>
```

浏览器界面如下：

{% asset_img 9.png %}