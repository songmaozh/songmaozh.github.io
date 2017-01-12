---
title: Html 标签
date: 2017-01-12 10:00:06
tags:
---
# HTML< a >标签

##  href 属性

href 属性规定链接的目标：
```python
<a href="http://www.w3school.com.cn">W3School</a>
```
定义和用法
href 属性用于指定超链接目标的 URL。
href 属性的值可以是任何有效文档的相对或绝对 URL，包括片段标识符和 JavaScript 代码段。如果用户选择了 < a > 标签中的内容，那么浏览器会尝试检索并显示 href 属性指定的 URL 所表示的文档，或者执行 JavaScript 表达式、方法和函数的列表。
提示和注释
注意：< a > 标签中必须提供 href 属性或 name 属性。


一个引用其他文档的简单 < a > 标签可以是下列形式：
```python
<a href="http://www.w3school.com.cn/index.html"> W3School 在线教程</a>
```
浏览器用特殊效果显示短语“W3School 在线教程”（通常是带下划线的蓝色文本），这样用户就会知道它是一个可以链接到其他文档的超链接。就像这样：
W3School 在线教程
用户还可以利用浏览器中的选项来自己指定文本颜色、设置链接前和链接后链接文本的颜色。
提示：可以使用 CSS 伪类向文本超链接添加复杂而多样的样式。

制作图像链接
更复杂的锚还可以包含图像。下面这个 LOGO 是一个图像链接，点击该图像，可以返回 W3school 的首页：
```python
<a href="http://www.w3school.com.cn/index.html">
<img src="/i/w3school_logo_white.gif" />
</a>
```
上面的代码会为 W3School 的 LOGO 添加一个返回首页的超链接：
W3School 在线教程
大多数图形浏览器都会在作为锚的一部分的图像周围放置特殊的边框。通过在 <img> 标签中把图像的 border 属性设置为 0 可以删除超链接的边框。也可以使用 CSS 的边框属性来全局性地改变元素的边框样式。
语法
```python
<a href="value">
```
属性值
值    描述
URL	超链接的 URL。可能的值：
绝对 URL - 指向另一个站点（比如 href="http://www.example.com/index.htm"）
相对 URL - 指向站点内的某个文件（href="index.htm"）
锚 URL - 指向页面中的锚（href="#top"）

文件名URL，可以不包含文件后缀
相对跳转有如下方式，需要了解（以下的例子中，分别以你的例子和带.html尾缀进行演示）：
1. 本目录的使用（与本文件在相同的文件夹下）：

```python
<a href="123456">
<a href="123456.html">
```
2. 本目录下的子文件夹（设文件夹名为newdoc）的使用：

```python
<a href="newdoc/123456">
<a href="newdoc/123456.html">
```
3. 本目录下的子文件夹下的子文件夹（设文件夹名为newdoc2）的使用(如果更多层，则依此类推)：

```python
<a href="newdoc/newdoc2/123456">
<a href="newdoc/newdoc2/123456.html">
```
4. 本目录上一层父目录的使用：

```python
<a href="../123456">
<a href="../123456.html">
```
5. 本目录上两层父目录的使用(如果更多层，则依此类推)：

```python
<a href="../../123456">
<a href="../../123456.html">
```
6. 本目录上一层父目录下一个名为new文件夹下的使用(也就是和本文件所在的文件夹在相同目录下的那个new文件夹)：

```python
<a href="../new/123456">
<a href="../new/123456.html">
```
## html中a标签href属性的一个坑
由于公司需要，小菜最近在搞app web开发，目前只有ios和android版本，虽然仅此两个版本，但是依然要考虑浏览器兼容性问题，因为android和ios默认浏览器内核是不一样的。

     先说说兼容性问题是什么。假如有这样一个URL：

          http://www.kpdown.com/search?name=Ben Nadel 

     此URL后边有一个name参数，只不过参数的值竟然带了空格，这样的链接，直接用android浏览器访问，是没有问题的，但用ios的浏览器访问，这就是一个错误的URL，会报错的！

     所以，我们会想到编码，name参数的值，可以用encodeURIComponent()方法进行编码，然后再拼接到URL上，这样就安全了（encodeURIComponent是js原生方法，直接用即可）。

     然后，我们可以这样利用超链接：
```python
           <a href="javascript:openURL('http://www.kpdown.com/search?name=Ben%20Nadel');" >查询</a> 
```
     利用openURL这个js方法进行页面跳转（假设有一个openURL方法，其中不涉及任何解码操作）。

     这段代码在android中运行正常，但到了ios中，依然报错，的确是编码了，为什么还是不行呢？

     请看如下代码：

```python
 <a href="javascript:openURL('http://www.kpdown.com/search?name=Ben%20Nadel');">测试href</a>
 <a href="javascript:;" onclick="javascript:openURL('http://www.kpdown.com/search?name=Ben%20Nadel');">测试onclick</a>
 
 <script>
   function openURL(url){
     /*
     * 测试href --print--> http://www.kpdown.com/search?name=Ben Nadel
     * 测试onclick --print--> http://www.kpdown.com/search?name=Ben%20Nadel
     */
     console.log(url);
   }
 </script>
```
 
由此可见：“万恶”的href属性，在调用openURL传参时自动解码，而onclick属性则保持参数原封不动。
因此，小菜强烈不推荐使用a标签的href属性调用js，onclick方法非常的科学，非常的稳定，非常的正确，href的本意就是用来跳转URL，就不要用它来执行js啦。其实更好的做法是绑定事件，那样代码更好管理，看起来也整洁。
