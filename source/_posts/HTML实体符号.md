---
title: HTML实体符号
date: 2020-06-17 21:51:22
tags:
- HTML
categories:
- HTML
mathjax: true
---

**HTML 中的预留字符必须被替换为字符实体**，HTML实体符号包含数学符号、希腊字母、各种箭头、技术符号及各种形状

所有的实体符号以&开始，以;结束

在 HTML 中，某些字符是预留的。

在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。

如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。

字符实体有两种方式：

```html
<!--实体名称-->
&entity_name;
<!--实体编号-->
&#entity_number;
```

**提示：**使用实体名而不是数字的好处是，名称易于记忆。不过坏处是，浏览器也许并不支持所有实体名称（对实体数字的支持却很好）。



下面举例几个实体符号：

- **大于号和小于号**

如果不使用实体符号，而是直接使用>或者<，将会导致与HTML预留符号冲突，例如：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML实体符号</title>
	</head>
	<body>
		a<b>c
	</body>
</html>
```

浏览器界面如下：

{% asset_img 10.png %}

可以看到，这里只显示了ac，因为中间的部分被识别为了标签而被解析，所以没有显示出来

那么如果要让他们显示出来，就需要使用**实体符号**

如下所示：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML实体符号</title>
	</head>
	<body>
		<!--&lt;表示小于号，&gt;表示大于号-->
		a&lt;b&gt;c
	</body>
</html>
```





- **不间断空格（non-breaking space）**

HTML 中的常用字符实体是不间断空格

浏览器总是会截短 HTML 页面中的空格。如果您在文本中写 10 个空格，在显示该页面之前，浏览器会删除它们中的 9 个。如需在页面中增加空格的数量，需要使用实体符号，如下所示：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>HTML实体符号</title>
	</head>
	<body>
		<!--&nbsp;代表空格-->
		<!--下面代码代表加了6个空格-->
		a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c
	</body>
</html>
```



其余的各种实体符号，只需要在需要使用的时候网上查阅即可！