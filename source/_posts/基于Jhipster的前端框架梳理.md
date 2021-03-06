---
title: 基于Jhipster的前端框架梳理
date: 2017-01-13 13:44:03
tags:
---

## 基于Jhipster的前端框架梳理


JHipster(以下简称JH) 根据鄙人这几天的理解, JHipster 不算是一门技术,而更多的算是各种最佳实践的结合,基于Yoeman来讲符合需求的技术架构的最佳实践生成出来, 并提供了通过JDL来自动生成Restful以及页面代码的方式. 这种可能很大程度上改变我们的开发模式.有以前的架构师定框架,写base类, 2~3年的程序员堆API的模式改成, 架构师根据需求通过JH来生成框架,再设计UML图出JDL 然后自动生成RestAPI 前端web,IOS 安卓调用的方式. 业务逻辑被前置,这也是自移动开发变得日益壮大后前端在单BS 架构上的变化. 

至于spring-boot 的目的是解放开发人员.java 开发经常会掉在jar包依赖(Maven 并没有很好的解决这个问题,Maven只是让你能管理你的依赖,至于依赖什么还是要靠你自己梳理,gradle没实际项目使用过,不过我觉得定位是一样的.Ant的话还要更灵活一点,但是也要更麻烦一点)各个模块的集成中(缓存,数据库,事务....)的深坑之中... 而spring-boot在一定程度上解决了这个问题, 依赖什么.怎么配置 spring-boot会把一些配置自动配置掉.而腾出更多的时间关注你的业务.

JHIpster在后端使用spring-boot的基础上把前端对于gulp, bower, npm, angular等技术的最佳实践来生产出来.为什么要用gulp,bower... 因为前端代码正在日新月异的变化.而浏览器都是按照ES3..ES5..ES2015 ES2016的规范来支持JS的,也就是说使用CoffeeJS,TypeScript开发的JS SCSS开发的样式文件不能被浏览器支持.需要我们在发布站点之前做一个编译的动作.将ts coffeejs翻译成浏览器能认识的方言.在这个编译的过程中我们还喜欢合并js文件,移除空格等来提示我们发布的站定的访问效率. 这些就是gulp,webpack,grunt灯要帮我们完成的任务. 而且在编译的时候我们会依赖angular依赖bootstrap等js 文件,而bower就是来帮我们处理这些事情(有点类似后天的maven 干的事情). 
关于Angular 截止目前为止JH貌似还不支持AngularJS2.0 (本人前台做的比较少,如有不对的地方敬请指出) 随着bootstrap等技术的发展前端的代码也通过MVVM之类的设计模式进行了业务代码和界面的分离.以前的jsp模板页面的技术在现在的web开发中已经很少见到应用了. 这是一个趋势.吧页面用bootsrap的风格做出来.把页面细节美化留给美工去完成.毕竟业务逻辑前置了,我还有一堆业务逻辑要等着去完成呢.


客户端技术栈

单页面Web应用:

响应式页面设计
HTML5 Boilerplate
Twitter Bootstrap
AngularJS
兼容 IE9+ 和其他现代浏览器
完整的国际化支持，基于 Angular Translate
可选 Sass 用于 CSS 设计
可选 Spring Websocket 来实现 WebSocket
强大的 Yeoman 开发工作流:

使用 Bower 可以轻松的安装 JavaScript 类库
使用 Gulp.js 构建, 优化项目, 支持 live reload
使用 Karma and PhantomJS 进行测试
那么，如果单页面应用不能满足你的需求呢？

支持 Thymeleaf 模板引擎, 用于在服务端渲染页面
 

 服务端技术栈

一个完整的 Spring 应用:

Spring Boot 用于简化应用配置
Maven 或者 Gradle 用于构建，测试和运行应用
"development" 和 "production" 配置文件 (支持 Maven 和 Gradle)
Spring Security
Spring MVC REST + Jackson
可选的 WebSocket 支持 -- 基于 Spring Websocket
Spring Data JPA + Bean 验证
使用 Liquibase 实现数据库自动更新
Elasticsearch 支持对数据库的搜索功能
支持像MongoDB 这样的 document-oriented NoSQL 数据库
支持像Cassandra 这样的 column-oriented NoSQL 数据库
支持生产环境：

Monitoring with Metrics 监控运行状态
支持 ehcache (本地缓存) 或者 hazelcast (分布式缓存)
可选的 HTTP session 集群 -- 基于 hazelcast
优化的静态资源(gzip filter, HTTP cache headers)
日志管理 Logback, 可在运行时配置
HikariCP 连接池，用于性能优化
可以将应用构建成一个标准的 WAR 文件或者一个可执行的 JAR 文件
 
 安装

安装前置条件

JDK 8+
Maven或者Gradle
NodeJs
PhantomJS(见下文安装说明)
MySql
Git
Spring Tool Suite或Eclipse或Intellij IDEA
window 管理员权限的 CMD或者PowerShell(推荐用PowerShell)
全局安装 Yeoman : npm install -g yo
全局安装 Bower:npm install -g bower
全局安装 Gulp ：npm install -g gulp-cli
全局安装 JHipster：npm install -g generator-jhipster
假如已经安装完毕则软件各版本如下

yo@1.8.5
bower@1.7.9
gulp-cli@1.2.2
npm@3.10.3
generator-jhipster@3.8.0
至此，JHipster已经安装完毕

## Yeoman学习与实践笔记

Yeoman是Google的团队和外部贡献者团队合作开发的，他的目标是通过Grunt（一个用于开发
任务自动化的命令行工具）和Bower（一个HTML、CSS、Javascript和图片等前端资源的包管理器）的包装为开发者创建一个易用的工作流。


Yeoman的目的不仅是要为新项目建立工作流，同时还是为了解决前端开发所面临的诸多严重问题，例如零散的依赖关系。

Yeoman主要有三部分组成：yo（脚手架工具）、grunt（构建工具）、bower（包管理器）。这三个工具是分别独立开发的，但是需要配合使用，来实现我们高效的工作流模式。

下面这幅图很形象的表明了他们三者之间的协作关系。
![image](http://images.cnitblog.com/blog/39469/201303/09214923-27fe6dea6eb34f468e601589ea83a675.png)
YOMAN的特性

闪电般的初始化：项目开始阶段，可以基于现有的模板框架（例如：HTML5 Bolierplate、Twitter Bootstrap）进行项目初始化的快速构建。
了不起的构建流程：不仅仅包括JS、CSS代码的压缩、合并，还可以对图片和HTML文件进行优化，同时对CoffeScript和Compass的文件进行编译。
自动编译CoffeScript和Compass：通过LiveReload进程可以对源文件发生的改动自动编译，完成后刷新浏览器。
自动Lint代码：对于JS代码会自动进行JSLint测试，确保代码符合最佳编程实践。
内置的预览服务器：不再需要自己配置服务器了，使用内置的就可以快速预览。
惊人的图片优化：通过使用OptiPNG和JPEGTran来优化图片，减少下载损耗。
杀手级包管理：通过bower search jQuery，可以快速安装和更新相关的文件，不再需要打开浏览器自己搜索了。
PhantomJS单元测试：可以非常方便的使用PhantomJS进行单元测试，一切在项目初始的时候都准备好了。

安装前的准备工作

检查系统中是否安装了：Node.js、Ruby、Compass。

Mac下安装Node.js非常方便，首页提供了一个pkg下载，双击后可以默认安装node、npm到/usr/local/bin下，我们只需要确保/usr/local/bin包含在PATH变量中就可以。

Mac Mountain Lion 下自带了Ruby，所以也就不需要再单独安装了。

Compass安装需要依赖于Ruby Gems，执行下面的步骤：
sudo gem update --system
sudo gem install compass

安装

环境准备好之后，就可以进行安装了，执行：
sudo npm install -g yo grunt-cli bower

http://www.cnblogs.com/cocowool/archive/2013/03/09/2952003.html

## bower

为什么使用bower，因为它可以节省掉你去git或是网上找js的时间
试着在项目文件夹下，下载jquery 和 underscore
bower install jquery underscore
然后就可以看到项目文件夹下多了一个app文件，里面有bower_components，再就去就是两个插件了包了
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=5)
初步这样也就行了，但是app/bower_components这个目录有点让人不习惯，我想把东西下载到我习惯的目录里。需要加一个.bowerrc文件。注意，不需要名字什么的，只要新增一个.bowerrc就行了。里面用可以定义下载目录
{
  "directory": "app/vendor"
}
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=6)
同样的cmd命令再执行一遍，这次可以看到文件下载到app/vendor中了
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=7)
如果已经下载了很多必要的js，然后又不小心vendor文件夹删了，或者说另一个项目也需要类似的配置，难道还要一个一个输入命令吗？为了方便我们还要再加一个bower.json配置文件
可以自己用文本编辑器新增一个，也可以用bower.init初始化
bower.init后，它会问你很多问题，一路默认就行了
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=8)
然后文件夹里就会多一个bower.json
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=9)
bower install --save handlebars 后就会看到handlebar 在bower.json的dependencies里，如果不加--save就不会有。
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=10)
接下来删了app/vendor下的所有内容，然后bower install，他会把bower.json中的dependencies重新下载
![image](http://jingyan.baidu.com/album/03b2f78c6bd7c05ea337ae6e.html?picindex=11)


##　AngularJS　Bootstrap
AngularJS依赖注入，JQuery依赖选择器；
AngularJS有助于前后台数据的耦合，而JQuery无法达到该效果；
目前华为公司采用的就是：内部前端框架(名称不能漏，不好意思，自己公司开发的)+AngularJS+Bootstrap。无论是从性能上，还是从前后台数据耦合程度而言，都比传统JQuery来得强；
我刚进华为，目前正在学习AngularJS中。
Bootstrap 和 Angular 都是人们大量使用的工具。在很多项目中，它们需要一起使用。为什么不呢？他们已经改变了CSS和JS的开发方式，让前端既成为令人难以置信的工具。
但是，把它们放在一起使用还有一些问题，特别是当你试图在Angular的项目中引入Bootstrap JavaScript组件时，会是一个问题。当建立了Angular的项目，##你不应该添加完整的jQuery库##。 jQlite已经包含在Angular中的，所有jQuery必要的功能它都有。这是因为把jQuery添加到Angular的项目将很难让你完全掌握Angular的核心优势和数据绑定的力量。
比如你想在某种程度上改变View视图，一个很好的做法是通过Angular所绑定的data数据来改变。我们写这篇文章的目标就是为了，学习用jQuery和Angular通过不同的手段达到相同的目的。
Bootstrap JavaScript和Angular问题
这个问题可以追溯到，你不应该在你的Angular项目中使用jQuery的原则。jQuery操作视图的方法与Angular操纵数据的方法会起冲突。
为什么你不应该使用jQuery
您使用jQuery操作数据抓住并注入到DOM的方式基本上基于事件。当我们使用Bootstrap JavaScript组件时，比如一个按钮，我们需要“单击此按钮时，设置此按钮为激活状态”。并将这种设置添加入新加的按钮中。通过添加 .active 类和检查input（如果你的按钮是一个input）为实现。
在Angular中，操纵数据不是通过抓取和注入。一般通过数据绑定来实现，野蛮抓取注入数据。也能够改变每个组件的状态，不过在切换时就会暴露出问题。
这就是为什么我们不能直接用Bootstrap的JavaScript。它依赖于jQuery我们不希望jQuery的破坏我们的Angular项目。如果我们试图绑定变量到组件，它无法工作。
回到顶部
解决方案： UI Bootstrap
那么该如何解决？我们从Angular得知，我们需要将数据绑定到一个特定的组件，我们应该建立一个directive 指令。这将让我们的Angular网站，更关注数据的变化。
该解决方案是一种被称为UI Bootstrap 的项目。这是由Angular UI团队开发的，增加了许多Angular的扩展组件。UI Bootstrap不使用jQuery; 它为每个Bootstrap JS组件添加了内置指令（directives）。
对于UI Bootstrap（非BootstrapJS）的要求是：
没有jQuery的依赖
依赖Angular
依赖Bootstrap CSS文件
就是这样。现在，我们要如何将它集成到我们的项目？
回到顶部
我们的Angular应用
让我们来看看我们需要些什么设置。如果你已经看过JavaScript代码，你会看到我们创建了一个Angular模块和控制器。然后，我们创建的按钮和折叠的变量。
为此，下一步就是要拿到UI Bootstrap的文件，放到我们的项目中。那么我们能够在我们的Angular模块中导入ui.bootstrap。就这样，我们就能够获取需要模仿BootstrapJS组件的指令（directives ）！


AngularJS Bootstrap
http://www.runoob.com/angularjs/angularjs-bootstrap.html

UI Bootstrap 是由 AngularJS UI 团队编写的纯 AngularJS 实现的 Bootstrap 组件。
