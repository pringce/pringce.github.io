---
title: HTML表格
date: 2020-07-02 16:55:50
tags:
- HTML
categories:
- HTML
mathjax: true
---

## 如何画一个表格

下面写一个**两行三列**的表格：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>表格</title>
	</head>
	<body>
		<!--table标签代表表格-->
		<!--tr标签代表一行，td标签代表一格-->
		<!--下面是一个两行三列的表格-->
		<table>
			<tr>
				<td>a</td>
				<td>b</td>
				<td>c</td>
			</tr>
			<tr>
				<td>d</td>
				<td>e</td>
				<td>f</td>
			</tr>
		</table>
	</body>
</html>
```

效果如下：

{% asset_img 1.png %}



**但是我们发现没有表格线，如何添加表格线呢？**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>表格</title>
	</head>
	<body>
		<!--
			border:表格边框宽度为1px
			width:宽度，有两种方式：1、直接写300px；2、百分比，代表占页面的宽度比
			height:高度，只能用px
			align:对齐方式
		-->
		<table align="center" border="1px" width="30%" height="150px">
            <!--该行数据居中对齐-->
			<tr align="center">
				<td>a</td>
				<td>b</td>
				<td>c</td>
			</tr>
			<tr>
				<td>d</td>
				<td>e</td>
                <!--该格数据居中对齐-->
				<td align="center">f</td>
			</tr>
		</table>
	</body>
</html>
```

效果如下：

{% asset_img 2.png}



## 单元格合并

**1、行合并**

**删除下面的单元格（注意不能删除上面的，只能删除下面的）**，最终只保留最上面的。然后在上面格内添加rowspan属性，合并几个单元格，rowspan的值就设置为几个

比如我们把上面的c和f合并，代码如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>表格</title>
    </head>
    <body>
        <table align="center" border="1px" width="30%" height="150px">
            <tr>
                <td>a</td>
                <td>b</td>
                <td rowspan="2">c</td>
            </tr>
            <tr>
                <td>d</td>
                <td>e</td>
            </tr>
        </table>
    </body>
</html>
```

效果如下：

{% asset_img 3.png %}



**2、列合并**

**删除单元格（删除哪几个都行），最后保留一个就行**。然后加入colspan属性

比如合并d和e，代码如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>表格</title>
    </head>
    <body>
        <table align="center" border="1px" width="30%" height="150px">
            <tr>
                <td>a</td>
                <td>b</td>
                <td>c</td>
            </tr>
            <tr>
                <td colspan="2">d</td>
                <td>f</td>
            </tr>
        </table>
    </body>
</html>
```

效果如下：

{% asset_img 4.png %}



## th标签

th标签也是单元格标签，比td多的是居中和加粗

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>表格</title>
    </head>
    <body>
        <table align="center" border="1px" width="30%" height="150px">
            <tr>
                <th>员工编号</th>
                <th>员工薪资</th>
                <th>部门名称</th>
            </tr>
            <tr>
                <td>a</td>
                <td>b</td>
                <td>c</td>
            </tr>
            <tr>
                <td colspan="2">d</td>
                <td>f</td>
            </tr>
        </table>
    </body>
</html>
```

效果如下：

{% asset_img 5.png}



## thead tbody tfoot

这个不是必须的，添加这个会使得操作表格更灵活，将表格分为三部分：表头、表体、表脚

这三个标签不加也可以，不会有什么影响。只是为了后期编写JS代码更方便操作表格

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>表格</title>
    </head>
    <body>
        <table align="center" border="1px" width="30%" height="150px">
            <!--头-->
            <thead>
                <tr>
                    <th>员工编号</th>
                    <th>员工薪资</th>
                    <th>部门名称</th>
                </tr>
            </thead>
            <!--体-->
            <tbody>
                <tr>
                    <td>a</td>
                    <td>b</td>
                    <td>c</td>
                </tr>
                <tr>
                    <td colspan="2">d</td>
                    <td>f</td>
                </tr>
            </tbody>
            <!--脚-->
            <tfoot>
                <tr>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                </tr>
            </tfoot>
        </table>
    </body>
</html>
```

