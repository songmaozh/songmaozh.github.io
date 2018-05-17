---
title: JSP & Html
date: 2017-02-21 14:05:10
tags:
---

jsp基于动态的方式编写，可以喝后台代码结合开发， 
而html是一种静态的，只能用静态的展示，如果想动态的并且结合html开发，那么就的用第三方开发框架结合html开发这样的功能

jsp中隐藏了request，response，session。你用html怎么交互数据这么方便呢？

HTML（Hypertext Markup Language）文本标记语言，它是静态页面，和JavaScript一样解释性语言，为什么说是解释性 语言呢？因为，只要你有一个浏览器那么它就可以正常显示出来，而不需要指定的编译工具，只需在TXT文档中写上HTML标记就OK。
JSP（Java Server Page）看这个意思就知道是Java服务端的页面，所以它是动态的，它是需要经过JDK编译后把内容发给客户端去显 示，我们都知道，Java文件编译后会产生一个class文件，最终执行的就是这个class文件，JSP也一样，它也要编译成class文件！JSP不 止要编译，它还得要转译，首先把JSP转译成一个Servlet文件，然后在编译成class文件。当用户访问JSP时就执行了class文件，最 终......

1.最简单的区别就是，HTML能直接打开，jsp只能发布到Tomact等服务器上才能打开 。
2.定义上HTML页面是静态页面可以直接运行，JSP页面是动态页它运行时需要转换成servlet。 
3.他们的表头不同，这个是JSP的头“ <%@ page language="java" import="java.util.*" pageEncoding="gbk"%>”在表头中有编码格式和倒入包等。
4.也是很好区分的在jsp中用<%%>就可以写Java代码了，而html没有<%%>。


jsp是动态网页（可动态更新页面上的内容），html是静态网页（固定内容，不会变）。jsp是java中一种动态网页开发技术，类似的技术有php、asp等。动态网页技术也是建立在html的基础上，它们是程序员编写的代码通过服务器动态生成，最后也是一个相应的html页面。

html是写静态页面的，数据是写死的，jsp是写动态页面的，数据是动态获取的


html可以使用 js获取浏览器内置对象XMLHttpRequest xhr； 通过xhr获取服务器端传来的数据     等撸主熟悉后会发现   只用servlet+html也能实现动态网页(好神奇)  


html 和 jsp都能哦，XMLHttpRequest是浏览器内置对象 
不过大多的都是使用JSP 动态页面生成技术 简单些啊。html估计能实现都够呛吧
不过现在的网页大量充斥JS代码 导致浏览器打开就300M内存不见了可见内存中js对象之多，真减轻了服务器的压力，I/O输出少量数据，刷新局部数据；不用生成整个html页面多划算的事
对于LZ的问题，html在应用服务器中还真很少用 动态页面生成必须理解哟
jsp来源：为减少servlet代码更简洁去掉out.println()表示层，出现了jsp由容器管理转化为.java文件编译做成servlet对象I/O出HTML静态页面，意思说只要你显示的数据必须到后台数据库取并且显示复杂就必须用到jsp   至于html(css+js<美工部分>)就交给美工吧

浏览器访问web时，看似是直接访问的jsp页面，其实:最先到达的地方是服务器，服务器创建好req和resp对象后再给jsp页面使用。在jsp中完成设置字符集和
取得表单参数后再调用servlet，完成业务处理。然后返回到jsp，jsp就会生成相应的html页面。该页面会返回到服务器，再由服务器，通过response对象返回给

http://img.bbs.csdn.net/upload/201305/27/1369618363_595343.png

当然可以，jsp页面还是会编译成servlet页面，jsp主要是可以方便初始化网页的数据， 

servlet去输出个html页面比较麻烦，再说了，你用html/ajax你的初始页面难道都是静态的， 

等页面加载完在用ajax异步调用？如果说有些数据是页面一出来就显示的还是用jsp比较快， 

jsp是在服务器端执行，输出在页面显示还是html/ajax。如果追求无刷新页面，可以在提交的时候 

使用ajax技术，后台用servlet接收(用jsp一样，如果你返回的是网页用jsp更方面)， 

其他页面初始化你用jsp，asp，asp.net，php都可以。

https://www.zhihu.com/question/48882201

网页设计的首屏尺寸一般是多少？
https://www.zhihu.com/question/23185173/answer/23848495
