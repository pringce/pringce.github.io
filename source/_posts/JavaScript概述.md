---
title: JavaScript概述
date: 2020-07-06 10:40:47
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

## 什么是JavaScript

JavaScript是运行在浏览器上的脚本语言，简称JS

JavaScript是网景公司（NetScape）的布兰登艾奇开发的，最初叫LiveScript

LiveScript的出现让浏览器更加生动，不再是单纯的静态页面了，更具有交互性

和Java没关系，Java运行在JVM当中，JavaScript运行在浏览器的内存当中

JavaScript不需要手动编译，编写完源代码之后，浏览器直接打开解释执行

JavaScript的**目标程序（能够被执行的程序）**以普通文本形式保存，这种语言都成为脚本语言

Java的目标程序以.class的形式存在，不能使用文本编辑器打开，不是脚本语言



LiveScript的出现，最初的时候是为Navigator浏览器（网景公司开发的）量身定做的，不支持其它浏览器。为此，微软开发了一种只支持IE浏览器的脚本语言，叫做JScript。

JavaScript和JScript语言并存的时代，程序员十分痛苦，因为要写两套程序。在这种情况下，一个非盈利性组织站了出来，叫做ECMA组织（欧洲计算机协会），ECMA根据JavaScript制定了ECMA-262标准，叫做**ECMA-Script**（简称ES），因此也可以认为现在的ES就是标准化的JavaScript语言



**JS代码的单行注释和多行注释方法与Java一样！！**



## JavaScript的组成

由三部分组成：ECMAScript，BOM，DOM。ECMAScript是**核心解释器（JS核心语法）**、DOM(Document Object Model)是**文档对象模型**、BOM(Browser Object Model)是**浏览器对象模型**



### ECMAScript

Javscript，JScript，ActionScript等脚本语言都是基于ECMAScript标准实现的。ECMAScript是一种由[Ecma国际](http://baike.baidu.com/view/3986646.htm)（前身为[欧洲计算机制造商协会](http://baike.baidu.com/view/2233504.htm),英文名称是European Computer Manufacturers Association）通过ECMA-262标准化的脚本程序设计语言。

它本身不包含输入输出定义；ECMA-262规定了语法、类型、语句、关键词、保留字、操作符、对象，ECMAScript就是对实现该规定的各个方面内容的语言的描述。javascript实现了ECMAScript。



### DOM

DOM 是“ Document Object Model ”的缩写，简称“ 文件对象模型 ”，由W3C制定规范。

DOM 定义了 JavaScript 操作 HTML 文档的接口，提供了访问 HTML 文档（如body、form、div、textarea等）的途径以及操作方法。

DOM树上的每一个节点都有唯一的id属性

对网页当中的节点进行增删改的过程，HTML文档被当做一棵DOM树来看待。例如：

```javascript
var domObj = document.getElementById("id");
```



### BOM

BOM 是“ Browser Object Model ”的缩写，简称“ 浏览器对象模型 ”。

BOM 定义了 JavaScript 操作浏览器的接口，提供了访问某些功能（**如浏览器窗口大小、版本信息、浏览历史记录、后退、关闭浏览器窗口、打开新窗口、浏览器地址栏上地址等**）的途径以及操作方法。

遗憾的是，BOM 只是 ECMAScript 的一个扩展，没有任何相关标准，W3C也没有对该部分作出规范，每个浏览器厂商都有自己的 BOM 实现，这可以说是 BOM 的软肋所在。

通常情况下，浏览器特定的（即非 W3C 标准规定的）JavaScript 扩展都被看作 BOM 的一部分，主要包括：

- 关闭、移动浏览器及调整浏览器窗口大小；
- 弹出新的浏览器窗口；
- 提供浏览器详细信息的定位对象；
- 提供载入到浏览器窗口的文档详细信息的定位对象；
- 提供用户屏幕分辨率详细信息的屏幕对象；
- 提供对cookie的支持；
- 加入ActiveXObject类扩展BOM，通过JavaScript实例化ActiveX对象。



### DOM和BOM的区别和联系

BOM的顶级对象是Window，DOM的顶级对象是Document

**实际上BOM是包含DOM的**，如图所示：

{% asset_img 1.png %}