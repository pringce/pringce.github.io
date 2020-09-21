---
title: HTML列表
date: 2020-07-04 11:39:48
tags:
- HTML
categories:
- HTML
mathjax: true
---

列表分为**有序列表**和**无序列表**



- 无序列表

```html
<ul>
    <li>中国
        <ul>
            <li>北京
            	<ul>
                    <li>海淀区</li>
                    <li>东城区</li>
                </ul>
            </li>
            <li>上海</li>
            <li>西安</li>
    	</ul>
    </li>
    <li>美国</li>
    <li>日本</li>
</ul>
```

**无序列表可以嵌套**

效果如下：

{% asset_img 1.png %}

看到坐标有小圆点、小方块等，可以设置每一级列表的标识，有三种：**circle、square和disc**。设置方式为：

```html
<ul type="square"></ul>
```



- 有序列表

```html
<ol>
    <li>中国
        <ol>
            <li>北京
            	<ol>
                    <li>海淀区</li>
                    <li>东城区</li>
                </ol>
            </li>
            <li>上海</li>
            <li>西安</li>
    	</ol>
    </li>
    <li>美国</li>
    <li>日本</li>
</ol>
```

**有序列表也可以嵌套**

效果如下：

{% asset_img 2.png %}

也可以设置前面的标识：比如a或A代表字母，1代表数字（默认），i或I代表罗马数字

```html
<ol type="a"></ol>
```

