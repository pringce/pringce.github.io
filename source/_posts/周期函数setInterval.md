---
title: 周期函数setInterval
date: 2020-07-13 10:59:04
tags:
- JavaScript
categories:
- JavaScript
mathjax: true
---

关于JS终内置的支持类：**Date，可以用来获取时间/日期**

- **获取系统当前时间**

```html
<body>
    <script type="text/javascript">
        //获取系统当前时间
        var nowTime = new Date();
        //输出
        document.write(nowTime);
    </script>
</body>
```

效果如下：

{% asset_img 1.png %}

调整时间输出格式：

```html
<script type="text/javascript">
    //获取系统当前时间
    var nowTime = new Date();
    //转换成具有本地语言环境的日期格式
    nowTime = nowTime.toLocaleString();
    document.write(nowTime);
</script>
```

输出效果如下：

{% asset_img 2.png %}

自定义输出格式：

```html
<script type="text/javascript">
    //获取系统当前时间
    var nowTime = new Date();
    //获取年
    var year = nowTime.getFullYear();
    //获取月,范围是0~11，需要加1
    var month = nowTime.getMonth();
    //获取日
    var day = nowTime.getDate();
    document.write(year+"年"+(month+1)+"月"+day+"日");
</script>
```

效果如下：

{% asset_img 3.png %}



- **获取毫秒数**

```html
<script type="text/javascript">
    //获取系统当前时间
    var nowTime = new Date();
    //获取毫秒数
    var time = nowTime.getTime();
    document.write(time);//毫秒数通常被用来当时间戳
</script>
```



下面来了一个**需求**

- 有一个显示系统时间按钮，点击之后在div中显示当前系统时间
- 同时，该时间会自动更新并显示，即每一秒改变一次
- 还有一个系统时间停止按钮，点击之后，系统时间会停止更新

代码如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>显示时间</title>
    </head>
    <body>
        <script type="text/javascript">
            function disPlayTime(){
                var time = new Date();
                time = time.toLocaleString();
                document.getElementById("timeDiv").innerHTML = time;
            }

            function start(){
                //setInterval是周期函数
                //每隔1000ms(1s)调用disPlayTime函数
                //res是全局变量
                res = window.setInterval("disPlayTime()",1000);
            }

            function stop(){
                //停止周期函数
                window.clearInterval(res);
            }
        </script>

        <input type="button" value="显示系统时间" onclick="start();" />
        <input type="button" value="系统时间停止" onclick="stop();" />
        <div id="timeDiv"></div>
    </body>
</html>
```

