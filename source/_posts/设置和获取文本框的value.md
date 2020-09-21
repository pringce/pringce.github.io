---
title: 设置和获取文本框的value
date: 2020-07-11 09:52:50
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

## 获取文本框的value

```html
<script>
	window.onload = function(){
        var btmElt = document.getElementById("btn").
        btn.onclick = function(){
            alert(document.getElementById("user").value)
        }
    }
</script>

<input type="text" id="user" />
<input type="button" id="btn" value="click!" />
```

还有一种情形：

```html
<!--这里也是获取文本框的value，this指的是文本框对象，blur是失去焦点事件-->
<input type="text" value="name" onblur="alert(this.value)" />
```



## 设置文本框的value

```html
<script>
	window.onload = function(){
        var btmElt = document.getElementById("btn").
        btn.onclick = function(){
            document.getElementById("user").value = "set name!"
        }
    }
</script>

<input type="text" id="user" />
<input type="button" id="btn" value="click!" />
```

