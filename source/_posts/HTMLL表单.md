---
title: HTMLL表单
date: 2020-07-03 10:49:30
tags:
- HTML
categories:
- HTML
mathjax: true
---

## 表单的作用

收集不同类型的用户输入。表单展现之后，用户填写表单，点击提交按钮提交数据给服务器



## < form > 标签

HTML 表单用于收集用户输入。

<form> 元素定义 HTML 表单：

**实例**

```html
<form>
     .
    form elements
     .
</form>
```



### Action属性

*action*属性定义向哪个服务器提交表单，用来指定服务器地址

***action*属性和超链接中的href属性一样，都可以向服务器发送请求**

向服务器提交表单的通常做法是使用**提交按钮**。

通常，表单会被提交到 web 服务器上的网页。

**注意：**如果省略 action 属性，则 action 会被设置为当前页面。



### Method 属性

*method*属性规定在提交表单时所用的 HTTP 方法（*GET* 或 *POST*）：

```html
<form action="action_page.php" method="GET">
```

或：

```html
<form action="action_page.php" method="POST">
```



**何时使用 GET？**

默认方法

如果表单提交是被动的（比如搜索引擎查询），并且没有敏感信息。

当您使用 GET 时，表单数据在页面地址栏中是可见的：

```
action_page.php?firstname=Mickey&lastname=Mouse
```

**注释：**GET 最适合少量数据的提交。浏览器会设定容量限制。



**何时使用POST？**

如果表单正在更新数据，或者包含敏感信息（例如密码）。

POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的。

采用post方式，表单提交数据的格式同get一样，区别在于是否显示出来



### < *form*>属性列表：

|      属性      |                           描述                           |
| :------------: | :------------------------------------------------------: |
| accept-charset |    规定在被提交表单中使用的字符集（默认：页面字符集）    |
|     action     |       规定向何处提交表单的地址（URL）（提交页面）        |
|  autocomplete  |         规定浏览器应该自动完成表单（默认：开启）         |
|    enctype     |        规定被提交数据的编码（默认：url-encoded）         |
|     method     |      规定在提交表单时所用的 HTTP 方法（默认：GET）       |
|      name      | 规定识别表单的名称（对于 DOM 使用：document.forms.name） |
|   novalidate   |                   规定浏览器不验证表单                   |
|     target     |       规定 action 属性中地址的目标（默认：_self）        |



## HTML 表单包含表单元素

表单元素指的是不同类型的 **input 元素、文本域、下拉列表**等等。

**注意：如果要正确地被提交，该表单元素必须设置一个 name 属性。否则无法被提交**

### input元素

最重要的表单元素是 < input >元素。

< input > 元素根据不同的 **type**属性，可以变化为多种形态。下面详细介绍一下< input >元素所有的输入类型

- **text**：*< input type="text" >*定义供文本输入的单行输入字段

```html
<form>
 	First name:<br>
    <input type="text" name="firstname">
    <br>
     Last name:<br>
    <input type="text" name="lastname">
</form> 
```

- **password**：*< input type="password" >* 定义密码字段：

```html
<form>
     User name:<br>
    <input type="text" name="username">
    <br>
     User password:<br>
    <input type="password" name="psw">
</form> 
```

**注意：**password 字段中的字符会被做掩码处理（显示为星号或实心圆）。

- **submit**：*< input type="submit" >* 定义提交表单数据至**表单处理程序**的按钮

value设置提交按钮的显示文本

表单处理程序（form-handler）通常是包含处理输入数据的脚本的服务器页面。

在表单form的 action 属性中规定表单处理程序（form-handler）

```html
<!--这里的action就是表单处理程序-->
<form action="action_page.php">
    First name:<br>
    <input type="text" name="firstname" value="Mickey">
    <br>
    Last name:<br>
    <input type="text" name="lastname" value="Mouse">
    <br><br>
    <input type="submit" value="Submit">
</form> 
```

**注意：**如果省略了提交按钮的 value 属性，那么该按钮将使用默认文本显示

- **reset**：*< input type="radio" >* 定义清空按钮

value设置清空按钮的显示文本

```html
<input type="reset"></input>
```

- **radio**：*< input type="radio" >* 定义单选按钮。单选按钮的value必须手动指定

```html
<form>
<input type="radio" name="sex" value="male" checked>Male
<br>
<input type="radio" name="sex" value="female">Female
</form> 
```

**注意**：checked表示默认选择了该单选按钮，此例中为男。如果不加checked，则不会默认选择

- **checkbox**：*< input type="checkbox">* 定义复选框，允许用户在有限数量的选项中选择**零个或多个**选项

```html
<form>
    <input type="checkbox" name="vehicle" value="Bike">I have a bike
    <br>
    <input type="checkbox" name="vehicle" value="Car">I have a car 
</form> 
```

**注意**：加checked可以默认选中，如

```html
<!--默认Bike被选中-->
<form>
    <input type="checkbox" name="vehicle" value="Bike" checked=true>I have a bike
    <br>
    <input type="checkbox" name="vehicle" value="Car">I have a car 
</form> 
```

- **button**：*<input type="button>* 定义**按钮**

```html
<input type="button" onclick="alert('Hello World!')" value="Click Me!">
```

点击之后，浏览器会弹出**helloworld！**的提示信息


**HTML5新增输入类型**

HTML5 增加了多个新的输入类型：

- color：*< input type="color">* 用于应该包含颜色的输入字段。根据浏览器支持，颜色选择器会出现输入字段中

```html
<form>
  Select your favorite color:
  <input type="color" name="favcolor">
</form>
```

- date：*< input type="date">* 用于应该包含日期的输入字段。根据浏览器支持，**日期选择器**会出现输入字段中。

```html
<form>
  Birthday:
  <input type="date" name="bday">
</form>
```

可以向输入添加限制

```html
<form>
  Enter a date before 1980-01-01:
  <input type="date" name="bday" max="1979-12-31"><br>
  Enter a date after 2000-01-01:
  <input type="date" name="bday" min="2000-01-02"><br>
</form>
```

这样，在日期选择当中只会出现符合限制条件的日期供选择

- datetime：*< input type="datetime">* 允许用户选择日期和时间（有时区）。根据浏览器支持，日期选择器会出现输入字段中

```html
<form>
  Birthday (date and time):
  <input type="datetime" name="bdaytime">
</form>
```

**注意：**Chrome、Firefox 或 Internet Explorer 不支持 type="datetime"。

- datetime-local：*< input type="datetime-local">* 允许用户选择日期和时间（无时区）。根据浏览器支持，日期选择器会出现输入字段中

```html
<form>
  Birthday (date and time):
  <input type="datetime-local" name="bdaytime">
</form>
```

**注意：**Firefox 或者 Internet Explorer 不支持 type="datetime-local"。

- email：*< input type="email">* 用于应该包含电子邮件地址的输入字段。根据浏览器支持，能够在被提交时自动对电子邮件地址进行**验证**

```html
<form>
  E-mail:
  <input type="email" name="email">
</form>
```

- month：*< input type="month">* 允许用户选择月份和年份。根据浏览器支持，日期选择器会出现输入字段中

```html
<form>
  Birthday (month and year):
  <input type="month" name="bdaymonth">
</form>
```

- number：*< input type="number" >* 用于应该包含数字值的输入字段。能够对数字做出限制，如果超出范围会提示超出范围

```html
<form>
  Quantity (between 1 and 5):
  <input type="number" name="quantity" min="1" max="5">
</form>
```

- range：*< input type="range">* 用于包含一定范围内的值的输入字段。根据浏览器支持，输入字段能够显示为滑块控件

```html
<form>
  <input type="range" name="points" min="0" max="10">
</form>
```

**注意**：能够使用如下属性来规定限制：min、max、step、value（**输入限制字段下面会介绍**）

- search：*< input type="search">* 用于搜索字段（**搜索字段的表现类似常规文本字段**）

```html
<form>
  Search Google:
  <input type="search" name="googlesearch">
</form>
```

- tel：*< input type="tel">* 用于应该包含电话号码的输入字段。**目前只有 Safari 8 支持 tel 类型**
- time：*< input type="time">* 允许用户选择时间（无时区）。根据浏览器支持，时间选择器会出现输入字段中

```html
<form>
  Select a time:
  <input type="time" name="usr_time">
</form>
```

- url：*< input type="url">* 用于应该包含 URL 地址的输入字段。根据浏览器支持，在提交时能够自动验证 url 字段

- week：*< input type="week">* 允许用户选择周和年。

  根据浏览器支持，日期选择器会出现输入字段中。

```html
<form>
  Select a week:
  <input type="week" name="week_year">
</form>
```

**注意：**老式 web 浏览器不支持的输入类型，会被视为输入类型 text

- file：*< input type="file">* 用于选择文件上传

```html
<!--添加multiple，允许选择多个文件上传-->
<input type="file" name="myfile" multiple="multiple">
```

- hidden：*< input type="file">* ，隐藏域，网页上看不到，但是表单提交时数据会自动提交给服务器

```html
<input type="hidden" name="userID" value="111">
```



**输入限制**

|   属性    |               描述               |
| :-------: | :------------------------------: |
| disabled  |      规定输入字段应该被禁用      |
|    max    |       规定输入字段的最大值       |
| maxlength |     规定输入字段的最大字符数     |
|    min    |       规定输入字段的最小值       |
|  pattern  | 规定通过其检查输入值的正则表达式 |
| readonly  |  规定输入字段为只读（无法修改）  |
| required  | 规定输入字段是必需的（必需填写） |
|   size    |  规定输入字段的宽度（以字符计）  |
|   step    |    规定输入字段的合法数字间隔    |
|   value   |       规定输入字段的默认值       |

**disabled和readonly的异同点：**

- 相同点：都是只读，不能修改
- 不同点：readonly可以提交给服务器，disabled数据不会提交（即使有name属性也不会提交）



### select元素

*< select>* 元素定义**下拉列表**：

```html
<select name="cars">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
    <option value="fiat">Fiat</option>
    <option value="audi">Audi</option>
</select>
```

*< option>* 元素定义待选择的选项。

列表通常会把首个选项显示为默认选项，即volvo。所以为了好看，通常可以这么写

```html
<select name="cars">
    <option value="">----请选择----</option>
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
    <option value="fiat">Fiat</option>
    <option value="audi">Audi</option>
</select>
```

您能够通过添加 selected 属性来定义默认选项。

```html
<option value="fiat" selected>Fiat</option>
```

此时默认选项为fiat

size可以设置默认显示的条数，默认是1

```html
<select name="cars" size="2"></select>
```

此时默认显示的条数就是2

**下拉列表如何支持多选？**

```html
<select name="cars" multiple="multiple"></select>
```

按住ctrl键便可实现多选



### textarea元素

*< textarea>* 元素定义多行输入字段（**文本域**）：

```html
<textarea name="message" rows="10" cols="30">
The cat was playing in the garden.
</textarea>
```

**注意**：rows代表行数，cols代表列数

### button元素

*< button>* 元素定义可点击的**按钮**：

```html
<button type="button" onclick="alert('Hello World!')">Click Me!</button>
```

和input元素中的type=button效果一样，点击之后会有弹窗！


### HTML5新增元素

- datalist：*< datalist>* 元素为 < *input*> 元素规定预定义选项列表。用户会在他们输入数据时看到预定义选项的下拉列表（相当于智能提醒，你在输入几个字母后会提示带匹配的选项）。< *input*> 元素的 *list* 属性必须引用 < *datalist*> 元素的 *id* 属性。

```html
<form action="https://www.baidu.com">
    <input type="text" list="browsers" name="browser">
    <datalist id="browsers">
        <option value="Internet Explorer">
        <option value="Firefox">
        <option value="Chrome">
        <option value="Opera">
        <option value="Safari">
    </datalist> 
    <input type="submit"></input>
</form>
```

- keygen
- output



## 用 < *fieldset*> 组合表单数据

*< fieldset>* 元素组合表单中的相关数据

当一组表单元素放到 < *fieldset*> 标签内时，浏览器会以特殊方式来显示它们，它们可能有特殊的边界

*< legend>* 元素为 < *fieldset*> 元素定义标题。

```html
<form action="action_page.php">
    <fieldset>
    	<legend>Personal information:</legend>
    	First name:<br>
    	<input type="text" name="firstname" value="Mickey">
    	<br>
    	Last name:<br>
    	<input type="text" name="lastname" value="Mouse">
    	<br><br>
    	<input type="submit" value="Submit">
    </fieldset>
</form> 
```



## 表单以何种格式提交数据给服务器

以name和value的键值对形式进行提交

格式：**action?name=value&name=value&name=value...**

例如：https://www.baidu.com?username=abc&pwd=111



**超链接和表单都是向服务器发送请求，但是表单会同时携带数据进行提交。**

**超链接也可以进行数据提交**：

```html
<a href="https://www.baidu.com?username=zhang&pwd=123">提交</a>
```

**注意**：超链接提交的数据都是固定不变的（在代码里写死了），**且超链接是get请求**



## 用户注册表单的实现

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>注册表单</title>
    </head>
    <body>
        <!--
            用户注册：用户名、姓名、密码、确认密码、性别、兴趣爱好、学历和简介
        -->
        <form action="https://www.baidu.com">
            用户名
            <input type="text" name="username"></input><br>
            密码
            <input type="text" name="pwd"></input><br>
            确认密码  <!--（不需要提交，浏览器就能判断）-->
            <input type="text"></input><br>
            性别
            <input type="radio" name="sex" value="male">男</input>
            <input type="radio" name="sex" value="female" >女</input><br>
            兴趣爱好
            <input type="checkbox" name="interest" value="football">足球</input>
            <input type="checkbox" name="interest" value="basketball">篮球</input><br>
            学历
            <select name="grade">
                <option value="gz">高中</option>
                <option value="bk">本科</option>
                <option value="sh">硕士</option>
            </select><br>
            简介 <!--文本域没有value属性，用户填写的内容就是value-->
            <textarea name="message" rows="10" cols="60"></textarea><br>
            <input type="submit" value="注册"></input>
            &nbsp;&nbsp;&nbsp;&nbsp;
            <input type="reset" value="清空"></input>
        </form>
    </body>
</html>
```

效果如下：

{% asset_img 1.png %}