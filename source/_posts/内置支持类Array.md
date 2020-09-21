---
title: 内置支持类Array
date: 2020-07-14 09:57:02
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

创建数组，有两种方式：

- **第一种**

```javascript
//创建数组长度为0的数组
var arr = [];
var array = [false,true,1,2,"abc"];
```

JS中的数组的数据类型随意，元素个数随意，可以**自动扩容**，这里和Java是有区别的

```javascript
var arr = [1,2,3,false,3.14];
arr[5] = "test";//这里不会报错，因为数组会自动扩容
```

**遍历数组**

```javascript
for(var i=0;i<array.length;i++){
    alert(array[i]);
}
```



- **第二种**

```javascript
//创建长度为0的数组
var a = new Array();
//创建长度为3的数组
var b = new Array(3);
//长度为2的数组，两个元素分别为3和2
var c = new Array(3,2);
```



**数组的常用函数**

```html
<script type="text/javascript">
    var a = [1,2,3];
    var str = a.join("-");//"1-2-3"

    //在数组的末尾添加一个元素(数组长度+1)
    a.push(10);

    //将数组末尾的元素弹出(数组长度-1)
    //JS的数组会自动模拟栈，后进先出
    var endElt = a.pop();

    //反转数组
    a.reverse();
</script>
```

