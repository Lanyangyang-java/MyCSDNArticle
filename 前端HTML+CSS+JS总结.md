
@[TOC](前端HTML CSS JS总结 )

---
# 一、HTML
## 1.HTML 介绍
**HTML(HyperText Markup Language)：超文本标记语言：**
**超文本：** 超越了文本的限制，比普通文本更强大。除了文字信息，还可以定义图片、音频、视频等内容，如上图看到的页面，我们除了能看到一些文字，同时也有大量的图片展示；有些网页也有视频，音频等。这种展示效果 超越了文本展示的限制。
**标记语言：** 由标签构成的语言。

**W3C 标准：** W3C 是万维网联盟，这个组成是用来定义标准的。他们规定了一个网页是由三部分组
成，分别是：
1. 结构：对应的是 HTML 语言
2. 表现：对应的是 CSS 语言
3. 行为：对应的是 JavaScript 语言

HTML 定义页面的整体结构；CSS 是用来美化页面，让页面看起来更加美观；JavaScript 可以使网页动起来，比如轮播图也就 是多张图片自动的进行切换等效果。

如同盖房子一样 HTML是房子的整体架构 CSS用于装修房子让房子看起来更美观  JavaScript让房子内增加许多功能
## 2.基础标签
1. 标题标签：通过`<h1><h6>`标签进行定义的。`<h1>` 定义最大的标题，`<h6>` 定义最小的标题。
2. `<hr>` 标签：浏览器中呈现出横线的效果
3. 字体标签：font：字体标签，font 标签已经不建议使用了，以后如果要改变文字字体，大小，颜色可以使用 CSS 进行设置。建议使用：
**face 属性：** 用来设置字体。如 "楷体"、"宋体"等
**color 属性：** 设置文字颜色。颜色有三种表示方式 英文单词 red, blue...这种方式表示的颜色特别有限，所以一般不用。rgb(值 1,值 2,值 3)：值的取值范围：0~255，此种方式也就是三原色(红绿蓝)设置方式。例如：rgb(255,0,0)。这种书写起来比较麻烦，一般不用。 #值 1 值 2 值 3：值的范围： 00~FF 这种方式是 rgb 方式的简化写法，以后基本都用此方
式。值 1 表示红色的范围，值 2 表示绿色的范围，值 3 表示蓝色范围。例如： #ff0000
**size 属性：** 设置文字大小
4. 换行标签：`<br>`
5. 段落标签：`<p></p>`
6. 加粗、斜体、下划线标签: `b`：加粗标签，`i`：斜体标签 `u`：下划线标签，在文字的下方有一条横线
7. 居中标签：`<center>`将 HTML 网页中的文本进行水平居中处理,不过HTML5 不支持` <center>`标签，用 CSS 代替。


**样例:**
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!-- 标题标签 -->
		<h1>我是标题 h1</h1>
		<h2>我是标题 h2</h2>
		<h3>我是标题 h3</h3>
		<h4>我是标题 h4</h4>
		<h5>我是标题 h5</h5>
		<h6>我是标题 h6</h6>
		<!-- 横线hr标签 -->
		<hr>
		<!-- 字体标签 -->
		<span style="font-family: 楷体; font-size: large; color: #ff0000; ">Web前端</span>
		
		<hr>
		<!-- 段落标签 -->
		Hello Web
		<p>
			Hellow HTML!
		</p>
		<p>
			段落标签
		</p>
		<!-- 换行标签 -->
		<br>
		<!-- 加粗、斜体、下划线标签 -->
		<b>加粗</b><br>
		<i>斜体</i><br>
		<u>下划线</u><br>
		
		<!--  居中标签 -->
		<center>居中标签</center>
		
	</body>
</html>
```
---
## 3.图片、音频、视频标签
 1. **img：定义图片** 
  src：规定显示图像的 URL (统一资源定位符)
  height：定义图像的高度
  width：定义图像的宽度
 2. **audio ：**定义音频。支持的音频格式： MP3、WAV、OGG
  src：规定音频的 URL
  controls：显示播放控件
3. video ：定义视频。支持的音频格式： MP4, WebM、OGG
  src：规定视频的 URL
  controls：显示播放控件
4. 路径
  绝对路径：完整路径，这里的绝对路径是网络中的绝对路径。 格式为：协议://ip 地址:端口号/资源名称
  相对路径：相对位置关系，找页面和其他资源的相对路径。./ 表示当前路径，../ 表示上一级路径../../ 表示上两级路径
  资源路径：图片，音频，视频标签都有 src 属性，而 src 是用来指定对应的图片，音频，视频文件的路径。此处的图片，音频，视频就称 为资源。资源路径有如下两种设置方式：
5. 尺寸单位：height 属性和 width 属性有两种设置方式：
像素：单x位是 px 百分比。占父标签的百分比。例如宽度设置为 50%，意思就是占它的父标签宽度的 50%

**样例:**
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!-- 这里的a图片，b音频，c视频均为网上下载到本地重新命名 -->
		<img src="../img/a.jpg" width="300" height="400">
		<!--在线链接，将图片网络地址拿到-->
		<img src="https://ts1.cn.mm.bing.net/th/id/R-C.9e45a633e95179a37c907fa2797999ad?
		rik=aMuPS4TunAh5ZA&riu=http%3a%2f%2fwww.quazero.com%2fuploads%2fallimg%2f140303%2f1-
		140303214Q2.jpg&ehk=P%2firfYpARc1fHht%2bWpapYR4W15p6SLABE8CBexoeon4%3d&risl=&pid=ImgRaw&r=0" width="300" height="400">
		<audio src="b.mp3" controls></audio>
		<video src="c.mp4" controls width="500" height="300"></video>
		</body>
	</body>
</html>
```
---
## 4.超链接标签
**超链接标签：** 
a 标签属性：
href：指定访问资源的 URL
target：指定打开资源的方式， _self：默认值，在当前页面打开， _blank：在空白页面打开

**样例：**
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<a href="https://www.jd.com" target="_self">京东</a>
		<a href="https://www.baidu.com" target="_blank">百度</a>
	</body>
</html>
```
## 5.列表标签

 **无序列表**
 使用 <ul> 标签
 ```html
 <ul>
	<li>咖啡</li>
	<li>牛奶</li>
</ul>
 ```
 **有序列表**
 使用<ol> 标签
 ```html
 <ol>
	<li>咖啡</li>
	<li>牛奶</li>
</ol>
```
浏览器中显示如下：
![请添加图片描述](https://i-blog.csdnimg.cn/direct/12c3ea35cc854a198c575552a7bfb4b4.png)
## 6.表格标签
**表格标签:**

标签名称|作用
-|-
 table |定义表格
 border|规定表格边框的宽度
 width |规定表格的宽度
 cellspacing|规定单元格之间的空白
 tr |定义行
 align|定义表格行的内容对齐方式
 td|定义单元格
 rowspan|规定单元格可横跨的行数
 colspan|规定单元格可横跨的列数
 th|定义表头单元格
 
 **样例：**
 ```html
 <!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<table border="1" cellspacing="0" width="500">
			<tr>
				<th>学号</th>
				<th>姓名</th>
				<th>年纪</th>
			</tr>
			<tr align="center">
				<td>001</td>
				<td>猪猪侠</td>
				<td>12</td>
			</tr>
			<tr align="center">
				<td>002</td>
				<td>蔡徐坤</td>
				<td>2.5</td>
			</tr>
			<tr align="center">
				<td>003</td>
				<td>灰太狼</td>
				<td>25</td>
			</tr>
		</table>
		<hr>
		<br>
		<table border="1" cellspacing="0" width="500">
			<tr>
				<th>学号</th>
				<th>姓名</th>
				<th>年纪</th>
			</tr>
			<tr align="center">
				<td>001</td>
				<td rowspan="2">猪猪侠</td>
				<td>12</td>
			</tr>
			<tr align="center">
				<td>002</td>
				<td >蔡徐坤</td>
				<td>2.5</td>
			</tr>
			<tr align="center">
				<td colspan="2">003</td>
				<td>灰太狼</td>
				<td>25</td>
			</tr>
		</table>
	</body>
</html>
 ```
## 7.布局标签
布局标签` <div>`、`<span>`这两个标签，一般都是和 css 结合到一块使用来实现页面的布局。
 div 标签 在浏览器上会有换行的效果，
 span 标签在浏览器上没有换行效果。
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<div>div</div>
		<div>div</div>
		<span>span</span>
		<span>span</span>
	</body>
</html>
```

## 8.表单标签
标签名称|作用
-|-
form |表示HTML表单action 表示表单提交的页面<br>method 表示表单的请求方式 ，有POST和GET两种，默认GET
input|表示用来收集用户输入数据的控件<br>通过设置type属性的不同属性值获取不同功能的控件
label|单选按钮 可以通过for属性绑定表单中其它控件，解释其他控件用途
select、option、optgroup| 表单下拉框组合<br>select下拉表单
textarea |表示可以输入多行文本的控件
datalist |定义一组提供给用户的建议值
fieldset、legend| 对表单中的控件分组
output| 表示计算结果
button 按钮|可以通过设置type值获取重置、提交、普通按钮这三种功能的按钮<br>这些功能的控件都可以通过input获取

 type属性
 属性名称|作用
-|-
  placeholder|字标
  text |账号
  password |密码
  date |数量
  file| 上传文件
  hidden |隐藏输入栏
  email|电子邮件
  radio| 单选框
  checkbox|多选框
  submit|提交
  action|属性提交地址
  method|get提交可见(默认提交方式)<br>post 提交不可见

**样例:**
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
	</body>
</html>
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<!-- 这里可以更换get和post查看导航栏的提交后的数据变化 -->
		<form action="#" method="get">
			<input type="hidden" name="id" value="123">
			<label for="username">用户名：</label>
			<input type="text" name="username" id="username"><br>
			<label for="password">密码：</label>
			<input type="password" name="password" id="password"><br>
			性别：
			<input type="radio" name="gender" value="1" id="male"> <label for="male">男</label>
			<input type="radio" name="gender" value="2" id="female"> <label for="female">女</label>
			<br>
			爱好：
			<input type="checkbox" name="hobby" value="1"> 旅游
			<input type="checkbox" name="hobby" value="2"> 电影
			<input type="checkbox" name="hobby" value="3"> 游戏
			<br>
			头像：
			<input type="file"><br>
			城市:
			<select name="city">
				<option>北京</option>
				<option value="shanghai">上海</option>
				<option>广州</option>
				<option>深圳</option>
			</select>
			<br>
			个人描述：
			<textarea cols="20" rows="5" name="desc"></textarea>
			<br>
			<br>
			<input type="submit" value="提交">
			<input type="reset" value="重置">
			<input type="button" value="一个按钮">
		</form>
	</body>
</html>
```
# 二、CSS
## 1.CSS 概述
CSS 是一门语言，用于控制网页表现，CSS 也有一个专业的名字： Cascading Style Sheet (层叠样式表)。
## 2.CSS 导入方式
1. **内联样式**
在标签内部使用 style 属性，属性值是 css 属性键值对该方式只能作用在这一个标签上，如果其他的标签也想使用同样的样式，那就需要在其他标签上写上相同的样式。复 用性太差。
 ```html
  <div style="color: red">Hello CSS~</div>
 ```
2. **内部样式：**
定义style标签，在标签内部定义 css 样式
```html
<style type="text/css">div{color: red;}</style>
```
3. **外部样式：**
  定义 link 标签，引入外部的 css 文件
```html
 <link rel="stylesheet" href="demo.css">
```
 **样例：**
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
		<style>
			span {
				color: yellow;
			}
		</style>
		<link href="./css/1.css" rel="stylesheet">
	</head>
	<body>
		<div style="color: red">hello css</div>
		<span>hello css </span>
		<p>hello css</p>
	</body>
</html>
```
1.css 中：
```css
p {
	color: blue;
}
```
---
## 3.CSS 选择器
1. 通用选择器
```css
* {
    margin: 0 auto;
    padding: 0;
}
```

 2. 标签选择器
`p`选择器能够匹配文档中所有的`< p>`标签。
```css
p {  
		color: blue;
}
  ```
3. ID选择器
 `#nav`选择器能够匹配文档中具有`id=" nav"`属性的标签。
```css
#nav {
		color: red;
 }
  ```
4. 类选择器
 `.black`选择器能够匹配文档中所有具有`class="black"`属性的标签。
 ```css
.black {
         color: black;
 }
  ```
5. 属性选择器
 `input[ type="text"]`选择器会匹配所有具有`type="text"`属性的`<input>`标签。
  属性选择器中方括号`[ ]`内的属性信息还支持以下几种写法：
  
```css
input[type="text"] { 
   color: blue;
}
```

[target]：选择所有带有`target`属性元素；
[target=_blank]：选择所有具有`target="_blank"` 属性的元素；
[title~=flower]：选择`title`属性包含单词“flower”的所有元素；
[lang|=en]：选择`lang`属性正好是“en”或以“en”为开头的所有元素。

6.  很少用的选择器
  分组选择器
  通用兄弟选择器
  相邻兄弟选择器
  子选择器
  后代选择器

**样例：** 这里列举几个常用的

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
		<style>
			div {
				color: red;
			}

			#name {
				color: blue;
			}

			.cls {
				color: yellow;
			}
		</style>
	</head>
	<body>
		<div>div1</div>
		<div id="name">div2</div>
		<div class="cls">div3</div>
		<span class="cls">span</span>
	</body>
</html>
```

---
# 三、JavaScript
## 1.JavaScript简介
1. JavaScript 是一门跨平台、面向对象的脚本语言，来控制网页行为的，它能使网页可交互
2. JavaScript 和 Java 是完全不同的语言，不论是概念还是设计。但是基础语法类似。
3. JavaScript（简称：JS） 在 1995 年由 Brendan Eich 发明，并于 1997 年成为一部 ECMA 标准。

---
## 2.JavaScript 引入方式
**内部脚本：将 JS代码定义在HTML页面中**
 在 HTML 中，JavaScript 代码必须位于 `<script>` 与 `</script>` 标签之间
 
```js
<script>
	alert("hello JS");
</script>
 ```
**提示:**
  1.在 HTML 文档中可以在任意地方，放置任意数量的`<script>`
  2.一般把脚本置于`<body>` 元素的底部，可改善显示速度，因为脚本执行会拖慢显示
  
**外部脚本：将 JS代码定义在外部 JS文件中，然后引入到 HTML页面中**
 外部文件：1.js
 引入外部 js文件
```html
<script src="./js/1.js"></script>
 ```

 **注意**
  外部脚本不能包含 `<script> `标签
`  <script>` 标签不能自闭合

**样例：**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>
<script>
    //alert("hello js");
</script>
<script src="./js/1.js"></script>
</body>
</html>
```
1.js
```js
alert("hello js");
```
---
## 3.JavaScript 基础语法
### 书写语法
1. 区分大小写：与 Java 一样，变量名、函数名以及其他一切东西都是区分大小写的
2. 每行结尾的分号可有可无
3. 注释格式很java注释格式一样
4. 大括号表示代码块 
```js
if (count == 3) {
	alert(count);
}
```
---
### 输出语句
1. 使用 window.alert() 写入警告框
2. 使用 document.write() 写入 HTML 输出
3. 使用 console.log() 写入浏览器控制台

```js
window.alert("hello JS");//弹出警告框
document.write("hello JS");//写入HTML
console.log("hello JS");//写入控制台
```
---
### 变量
1. JavaScript 中用 var 关键字（variable 的缩写）来声明变量
```js
var age = 20;
age = "张三";
```
2. JavaScript 是一门弱类型语言，变量可以存放不同类型的值
3. 变量名需要遵循如下规则：
组成字符可以是任何字母、数字、下划线（_）或美元符号（$）
数字不能开头
建议使用驼峰命名
4. let 关键字也能定义变量，的用法类似于 var，但是所声明的变量，只在 let 关键字所在的代码块内有效，且不允许重复声明。
5. const关键字用来声明一个只读的常量。一旦声明，常量的值就不能改变。

---
### 数据类型
 **原始类型**
  类型名称|说明
 -|-
  number|数字（整数、小数、NaN(Not a Number)）
  string|字符、字符串，单双引皆可
  boolean|布尔。true，false
  null|对象为空
  undefined|当声明的变量未初始化时，该变量的默认值是 undefined
  
 **引用类型**
 类型名称|说明
 -|-
Object| 对象
Array |数组
Function|函数

使用·`typeof `运算符可以获取数据类型
```js
alert(typeof age);
```
---
### 运算符
名称|符号
-|-
一元运算符|++，--
算术运算符|+，-，*，/，%
赋值运算符|=，+=，-=…
关系运算符|>，<，>=，<=，!=,==，`===`
逻辑运算符|&&，||，!
三元运算符|条件表达式 ? true_value : false_value


**类型转换：**
`==` 和 `=== `：
`==` 会进行类型转换，`===` 不会进行类型转换
```js
var age1=20;
var age2="20";
alert(age1==age2);//true
alert(age1===age2);//false
```

其他类型转为数字：
1. string:将字符串字面值转为数字。如果字面值不是数字，则转为NaN。一般使用parseInt方法进行转换
2. boolean:true 转为1，false 转为0

其他类型转为 boolean:
1. number：0和NaN转为false，其他的数字转为true
2. string：空字符串转为false，其他字符串转为true
3. null:转为false
4. undefined：转为false
### 流程控制语句
和java中的相似，这里不进行解释了，我的 [java基础总结上](https://blog.csdn.net/qq_60972641/article/details/142644807?spm=1001.2014.3001.5501)这篇文章有详细介绍
基本使用的就这五种
if ， switch， for，  while，  do…while

---
### 函数
函数（方法）是被设计为执行特定任务的代码块
  **JavaScript 函数通过 function 关键词进行定义**
 **定义方式一**
  **语法为：**
```js
function functionName(参数1,参数2..){
	//要执行的代码
}
  ```
**注意：**
1. 形式参数不需要类型。因为JavaScript是弱类型语言
2. 返回值也不需要定义类型，可以在函数内部直接使用return返回即可
  调用：函数名称(实际参数列表);
```js
function add(a, b){
	return a + b;
}
```

**调用：**
函数名称(实际参数列表)
```js
let result = add(1,2);
```

 **定义方式二：**
**语法为：**
```js
var functionName = function (参数列表){
	//要执行的代码
}

var add = function (a,b) {
	return a + b;
}
```
**调用：**
JS中，函数调用可以传递任意个数参数
```js
let result = add(1,2,3);
```
---
## 4.JavaScript 常用对象
### Array
**JavaScript Array对象用于定义数组**
 **定义:**
```js
//方式一 
var 变量名 = new Array(元素列表);
var arr = new Array(1,2,3);
//方式二
var 变量名 = [元素列表]; 
var arr = [1,2,3];
```
 **访问:**
```js
arr[索引] = 值;
arr[0] = 1;
```
 
 **注意：JS数组类似于Java集合，长度，类型都可变**
 
**对象属性**

属性|描述
-|-
 constructor 返回对创建此对象的数组函数的引用。
 length 设置或返回数组中元素的数目。
 prototype 使您有能力向对象添加属性和方法。
 
**Array 对象方法**
方法|描述
-|-
 concat() |连接两个或更多的数组，并返回结果。
 join()| 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
 pop()|删除并返回数组的最后一个元素
 push() |向数组的末尾添加一个或更多元素，并返回新的长度
 reverse() |颠倒数组中元素的顺序
 shift() |删除并返回数组的第一个元素
 slice() |从某个已有的数组返回选定的元素
 sort() |对数组的元素进行排序
 splice()| 删除元素，并向数组添加新元素。
 toSource() |返回该对象的源代码。
 toString()| 把数组转换为字符串，并返回结果。
 toLocaleString()| 把数组转换为本地数组，并返回结果
 unshift() |向数组的开头添加一个或更多元素，并返回新的长度。
 valueOf() |返回数组对象的原始值

---
### String
**定义**
```js
//方式一 
var 变量名 = new String(s); 
var str = new String("hello");
//方式二
var 变量名 = s; 
var str = "hello" ;
```

 **属性**
 属性|描述
-|-
  length|字符串的长度
  
 **方法**
 方法|描述
-|-
  charAt()|返回指定位置的字符
  IndexOf|检索字符串

---
### 自定义对象
**格式**
```js
var 对象名称 = {
		属性名称1:
		属性值1, 
		属性名称2:
		属性值2,... 
		函数名称:function (形参列表){
		}..
 };
```
 
**示例**
```js
var person = {
	name:"蔡徐坤",
	age:2.5, 
	eat:function (){
		alert("唱，跳，rap，篮球");
	}
};
```
---
## 5.BOM（浏览器对象模型）
### BOM简介
1. Browser Object Model 浏览器对象模型
2. JavaScript 将浏览器的各个组成部分封装为对象

**组成**
名称|说明
-|-
 Window|浏览器窗口对象
 History|历史记录对象
 Location|地址栏对象
 Navigator|浏览器对象
 Screen|屏幕对象

---

### Window浏览器窗口对象
**获取**
  直接使用 window，其中window. 可以省
  
 **属性：** 获取其他 BOM对象
  history对History对象的只读引用。
  Navigator对象的只读引用。
  Screen对Screen对象的只读引用。
  location用于窗口或框架的Location对象。
  
 **方法**
 方法名称|说明
 -|-|
  alert()|显示带有一段消息和一个确认按钮的警告框。
  confirm() |显示带有一段消息以及确认按钮和取消按钮的对话框。
  setInterval()| 按照指定的周期（以毫秒计）来调用函数或计算表达式。
  setTimeout() |在指定的毫秒数后调用函数或计算表达式。
**样例：**
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<script>
			window.alert("aaa");
			//方法：confirm点击确定返回是true,点击取消返回是false
			var flag = confirm("确认删除");
			alert(flag);
			
			setTimeout(function (){
			     alert("hello");
			 },2000);
			setInterval(function() {
				alert("hello");
			}, 2000);
		</script>
	</body>
</html>
```

---
###  Location(地址栏对象)
**获取**
使用 window.location获取，其中window. 可以省略
```js
window.location.方法();
location.方法()
```
 **属性**
href:设置返回完整的URL


**样例：**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    //location.href="https://www.baidu.com";
    // 3秒跳转到首页
    document.write("3秒跳转到首页");
    setTimeout(function (){
        location.href="https://www.baidu.com";
    },3000);

</script>
</body>
</html>
```
---
### History(历史记录)
**获取**
使用 window.history获取，其中window. 可以省略
```js
window.history.方法();
history.方法();
```
**方法**
  方法名称|说明
 -|-
  back（）|加载histor列表前一个URL
  forward()|加载histor列表下一个URL
  
 
---
## 6.DOM(文档对象模型)
Document Object Model 文档对象模型
将标记语言的各个组成部分封装为对象
**组成**
名称|说明
-|-
 Document|整个文档对象
 Element|元素对象
 Attribute|属性对象
 Text|文本对象
 Comment|注释对象

**DOM树：**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1602dd8c9a524f449811f7d6289cb44d.png)


**JavaScript 通过 DOM对 HTML进行操作**

1. 改变 HTML 元素的内容
2.  改变 HTML 元素的样式（CSS）
3.  对 HTML DOM 事件作出反应
4.  添加和删除 HTML 元素

### W3C DOM 标准
 **W3C DOM 标准被分为 3 个不同的部分：**
1. 核心 DOM：针对任何结构化文档的标准模型

  模型名称|范围
 -|-
   Document|整个文档对象
   Element|元素对象
   Attribute|属性对象
   Text|文本对象
   Comment|注释对象
2. XML DOM： 针对 XML 文档的标准模型
3. . HTML DOM： 针对 HTML 文档的标准模型
   Image：`<img>`
   Button ：`<input type=‘button’>`

### 获取 Element(元素对象)
 **Element：元素对象**
 **获取**：使用Document对象的方法来获取
 方法名称|说明
 -|-
getElementById|根据id属性值获取，返回一个Element对象
getElementsByTagName|根据标签名称获取，返回Element对象数组
getElementsByName|根据name属性值获取，返回Element对象数组
getElementsByClassName|根据class属性值获取，返回Element对象数组

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<img id="imgs" src="./img/1.jpg" width="400px"> <br>
		<div class="cls">div块1</div>
		<br>
		<div class="cls">div块2</div>
		<br>
		<input type="checkbox" name="hobby"> 电影
		<input type="checkbox" name="hobby"> 旅游
		<input type="checkbox" name="hobby"> sss游戏
		<br>
	</body>
	<script>
		//1.getElementyId根据id获取，返回的是一个Element对象
		var img = document.getElementById("imgs");
		alert(img);
		//2.getElementsByName根据name属性获取值，返回是Element对象数组
		var hobbys = document.getElementsByName("hobby");
		for (let i = 0; i < hobbys.length; i++) {
			alert(hobbys[i]);
		}
		//3.getElementsByTagName根据标签名称获取，返回的是Element对象数组
		var divs = document.getElementsByTagName("div");
		alert(divs.length);
		for (let i = 0; i < divs.length; i++) {
			alert(divs[i]);
		}
		//4.getElementsByClassName根据class属性值获取，返回的是一个Element对象数组
		var clss = document.getElementsByClassName("cls");
		for (let i = 0; i < clss.length; i++) {
			alert(clss[i]);
		}
	</script>
</html>
```

 ##  7.事件监听

### 事件绑定
**方式一
 通过 HTML标签中的事件属性进行绑定**
```js
<input type="button" onclick='on()’>
function on(){
 	alert("我被点了");
}
```
**方式二
 通过 DOM 元素属性绑定**
```js <input type="button" id="btn">
document.getElementById("btn").onclick = function (){
	alert("我被点了");
}
```
**样例：**
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>
	</head>
	<body>
		<input type="button" value="方式一" onclick="on()"><br>
		<input type="button" value="方式二" id="btn"><br>

		<script>
			function on() {
				alert("我被点击了");
			}
			document.getElementById("btn").onclick = function() {
				alert("我也被点击了");
			}
		</script>
	</body>
</html>
```
### 常见事件
事件名称|事件作用
-|-
onclick|鼠标单击事件
onblur|元素失去焦点
onfocus|元素获得焦点
onload|某个页面或图像被完成加载
onsubmit|当表单提交时触发该事件
onkeydown|某个键盘的键被按下
onmouseover|鼠标被移到某元素之上
onmouseout|鼠标从某元素移开


Event 代表事件对象

绑定绑定样例 表单验证
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>表单验证：欢迎注册</title>
	</head>
	<body>
		<div class="form-div">
			<div class="reg-content">
				<h1>欢迎注册</h1>
			</div>
			<form id="reg-form" action="#" method="get">
				<table>
					<tr>
						<td>用户名</td>
						<td class="inputs">
							<input name="username" type="text" id="username">
							<br>
							<span id="username_err" class="err_msg" style="display: none">用户名格式有误</span>
						</td>
					</tr>
					<tr>
						<td>密码</td>
						<td class="inputs">
							<input name="password" type="password" id="password">
							<br>
							<span id="password_err" class="err_msg" style="display: none">密码格式有误</span>
						</td>
					</tr>
					<tr>
						<td>手机号</td>
						<td class="inputs"><input name="tel" type="text" id="tel">
							<br>
							<span id="tel_err" class="err_msg" style="display: none">手机号格式有误</span>
						</td>
					</tr>
				</table>
				<div class="buttons">
					<input type="submit" value="提交注册" id="sub">
				</div>
				<br class="clear">
			</form>
		</div>

		<script>
			//验证用户名
			var usernameInput = document.getElementById("username");
			usernameInput.onblur = checkUsername;
			function checkUsername() {
				var username = usernameInput.value.trim();
				//正则表达式
				var reg = /^\w{6,12}$/;
				var flag = reg.test(username);
				if (flag) {
					document.getElementById("username_err").style.display = "none";
				} else {
					document.getElementById("username_err").style.display = "";
				}
			}
			//验证密码
			var passwordInput = document.getElementById("password");
			passwordInput.onblur = checkpassword;
			function checkpassword() {
				var password = passwordInput.value.trim();
				var reg = /^\w{6,12}$/;
				var flag = reg.test(username);
				if (flag) {
					document.getElementById("password_err").style.display = "none";
				} else {
					document.getElementById("password_err").style.display = "";
				}
			}
			//验证手机号
			var telInput = document.getElementById("tel");
			telInput.onblur = checktel;
			function checktel() {
				var tel = telInput.value.trim();
				var reg = /^[1]\d{10}$/;
				var flag = reg.test(tel);
				if (flag) {
					document.getElementById("tel_err").style.display = "none";
				} else {
					document.getElementById("tel_err").style.display = "";
				}
			}
			document.getElementById("sub").onclick = function() {
				alert("提交成功");
			}
		</script>
	</body>
</html>
```
