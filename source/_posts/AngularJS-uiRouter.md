---
title: AngularJS uiRouter
date: 2017-01-18 09:37:22
tags:
---
angular的uiRouter服务学习(5) --- $state.includes()方法

$state.includes方法用于判断当前激活状态是否是指定的状态或者是指定状态的子状态.

$state.includes(stateOrName,params,options)

$state.includes方法接受三个参数,其中第二和第三个都不知道是干啥的...估计也不太用得到,就暂时不管了...

stateOrName:字符串(必填). 是一个状态的名字.

比如当前的激活状态是 "contacts.details.item" 

如下调用:

$state.includes("contacts");                              //返回true
$state.includes("contacts.details");                      //返回true
$state.includes("contacts.details.item");                 //返回true
$state.includes("detail");                                //返回undefined
$state.includes("item");                                  //返回undefined
也可以使用glob语法:

复制代码
$state.$current.name = 'contacts.details.item.url';
 
$state.includes("*.details.*.*"); // returns true
$state.includes("*.details.**"); // returns true
$state.includes("**.item.**"); // returns true
$state.includes("*.details.item.url"); // returns true
$state.includes("*.details.*.url"); // returns true
$state.includes("*.details.*"); // returns undefined
$state.includes("item.**"); // returns undefined
复制代码
可以用于激活某个tab,让当前项高亮显示:

<li ng-class="{active:state.includes('dashboard.report')}"><a ui-sref="dashboard.report">Reports</a></li>
需要注意的是,在表达式里直接用$state是不行的,需要在控制器中把$state赋值给$scope下的变量.这样在表达式里才能使用:

复制代码
    $stateProvider.state('dashboard',{
        url:'/dashboard',
        templateUrl:'./tpls/dashboard.html',
        controller:function($scope,$state){
            $scope.state = $state;              
        }
    })
复制代码


ng-class是增加相关样式，可以和class同时使用

class说明是一个类，class=“active”本身这句是html代码，如果在css里设置样式应该在类名前加个点，如.active{}，从经验看，active这个类一般用在导航条中当前高亮的栏目，或者选项卡中当前活动着的选项

Angular-UI-Router里的指令ui-sref-active 查看当前激活状态并设置 Class
<li ui-sref-active="active"><a ui-sref="about">About</a></li>

上一篇中讲到使用$http同服务器进行通信，但是功能上比较简单，AngularJS还提供了另外一个可选的服务$resource,使用它可以非常方便的同支持restful的服务单进行数据交互。

五种默认行为：

{

　　“get”:{method:“get”},

　　“save”:{method:“post”}

　　“query”:{method:“get”,isArray:true}

　　“remove”:{method:“delete”}

　　“delete”:{method:“delete”}

}

首先给大家介绍angular-ui-router的基本用法。
如何引用依赖angular-ui-router?

angular.module('app',["ui.router"])
.config(function($stateProvider){
$stateProvider.state(stateName, stateCofig);
})

$stateProvider.state(stateName, stateConfig)
stateName是string类型
stateConfig是object类型
//statConfig可以为空对象
$stateProvider.state("home",{});
//state可以有子父级
$stateProvider.state("home",{});
$stateProvider.state("home.child",{})
//state可以是链式的
$stateProvider.state("home",{}).state("about",{}).state("photos",{});
stateConfig包含的字段：template, templateUrl, templateProvider, controller, controllerProvider, resolve, url, params, views, abstract, onEnter, onExit, reloadOnSearch, data

$urlRouteProvider
$urlRouteProvider.when(whenPath, toPath)
$urlRouterProvider.otherwise(path)
$urlRouteProvider.rule(handler)

$state.go
$state.go(to, [,toParams],[,options])
形参to是string类型，必须，使用"^"或"."表示相对路径；
形参toParams可空，类型是对象；
形参options可空，类型是对象，字段包括：location为bool类型默认true,inherit为bool类型默认true, relative为对象默认$state.$current,notify为bool类型默认为true, reload为bool类型默认为false
$state.go('photos.detail')
$state.go('^')到上一级,比如从photo.detail到photo
$state.go('^.list')到相邻state,比如从photo.detail到photo.list
$state.go('^.detail.comment')到孙子级state，比如从photo.detail到photo.detial.comment

ui-sref
ui-sref='stateName'
ui-sref='stateName({param:value, param:value})'

ui-view
==没有名称的ui-view
<div ui-view></div>
$stateProvider.state("home",{
template: "<h1>hi</h1>"
})
或者这样配置：

$stateProvider.state("home"{
views: {
"": {
template: "<h1>hi</h1>"
}
}
})
==有名称的ui-view

<div ui-view="main"></div>
$stateProvider.state("home",{
views: {
"main" : {
template: "<h1>hi</h1>"
}
}
})
==多个ui-view

<div ui-view></div>
<div ui-view="data"></div>
$stateProvider.state("home",{
views: {
"":{template: "<h1>hi</h1>"},
"data": {template: "<div>data</div>"}
}
})

项目文件结构
node_modules/
partials/
.....about.html
.....home.html
.....photos.html
app.js
index.html

创建state和view
app.js

var photoGallery = angular.module('photoGallery',["ui.router"]);
photoGallery.config(function($stateProvider, $urlRouterProvider){
$urlRouterProvider.otherwise('/home');
$stateProvider
.state('home',{
url: '/home',
templateUrl: 'partials/home.html'
})
.state('photos',{
url: '/photos',
templateUrl: 'partials/photos.html'
})
.state('about',{
url: '/about',
templateUrl: 'partials/about.html'
})
})

index.html

<!DOCTYPE html>
<html lang="en" ng-app="photoGallery">
<head>
<meta charset="UTF-8">
<title></title>
<link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.css"/>
</head>
<body>
<h1>Welcome</h1>
<div ui-view></div>
<script src="node_modules/jquery/dist/jquery.js"></script>
<script src="node_modules/angular/angular.js"></script>
<script src="node_modules/angular-ui-router/release/angular-ui-router.js"></script>
<script src="node_modules/angular-animate/angular-animate.js"></script>
<script src="node_modules/bootstrap/dist/js/bootstrap.js"></script>
<script src="node_modules/angular-bootstrap/ui-bootstrap-tpls.js"></script>
<script src="app.js"></script>
</body>
</html>


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
<!DOCTYPE html>
<html lang="en" ng-app="photoGallery">
<head>
<meta charset="UTF-8">
<title></title>
<link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.css"/>
</head>
<body>
<h1>Welcome</h1>
<div ui-view></div>
<script src="node_modules/jquery/dist/jquery.js"></script>
<script src="node_modules/angular/angular.js"></script>
<script src="node_modules/angular-ui-router/release/angular-ui-router.js"></script>
<script src="node_modules/angular-animate/angular-animate.js"></script>
<script src="node_modules/bootstrap/dist/js/bootstrap.js"></script>
<script src="node_modules/angular-bootstrap/ui-bootstrap-tpls.js"></script>
<script src="app.js"></script>
</body>
</html>

state之间的跳转
index.html
<nav class="navbar navbar-inverse">
<div class="container-fluid">
<div class="navbar-header">
<button class="navbar-toggle collapsed" type="button" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
<span class="icon-bar"></span>
<span class="icon-bar"></span>
<span class="icon-bar"></span>
</button>
<a ui-sref="home" class="navbar-brand">Home</a>
</div>
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
<ul class="nav navbar-nav">
<li>
<a ui-sref="photos">Photos</a>
</li>
<li>
<a ui-sref="about">About</a>
</li>
</ul>
</div>
</div>
</nav>
<div ui-view></div>

以上通过ui-sref属性完成state之间的跳转。

多个view以及state嵌套

有时候，一个页面上可能有多个ui-view，比如：

<div ui-view="header"></div>
<div ui-view="body"></div>
假设，以上页面属于一个名称为parent的state中。
我们知道在ui-router中，一个state大致是这样设置的：

<div ui-view="header"></div>
<div ui-view="body"></div>

所有state下views下的所有键值对(类似 "body@content":{templateUrl: 'partials/photos.html'})都被放到一个键值集合中。而ui-view的工作原理就是根据自己的属性值，到这个键值集合中去找匹配的键，找到就把对应的页面显示出来。
点击header对应的页面链接，可能会跳转到另外的子页面出现在<div ui-view="body"></div>这个位置。这时候页面出现了子父关系，而每个页面都属于某个state,这样state间就出现了子父关系。这些跳转的子页面，在路由设置中，可能被称为parent.son1, parent.son2...这就是state的嵌套。
在现有的文件结构上增加content.html, header.html,文件结构变为：
node_modules/
partials/
.....about.html
.....home.html
.....photos.html
.....content.html
.....header.html
app.js
index.html

content.html 包含了多各ui-view, 一个ui-view和页头相关，保持不变；令一个ui-view和会根据页头上的点击呈现不同的内容
<div ui-view="header"></div>
<div ui-view="body"></div>

header.html 把原先indext.html中nav部分放到这里来
<nav class="navbar navbar-inverse">
<div class="container-fluid">
<div class="navbar-header">
<button class="navbar-toggle collapsed" type="button" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
<span class="icon-bar"></span>
<span class="icon-bar"></span>
<span class="icon-bar"></span>
</button>
<a ui-sref="content.home" class="navbar-brand">Home</a>
</div>
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
<ul class="nav navbar-nav">
<li>
<a ui-sref="content.photos">Photos</a>
</li>
<li>
<a ui-sref="content.about">About</a>
</li>
</ul>
</div>
</div>
</nav>

index.html 这时变成了这样
<div ui-view></div>
app.js 路由现在这样设置
?
var photoGallery = angular.module('photoGallery',["ui.router"]);
photoGallery.config(function($stateProvider, $urlRouterProvider){
$urlRouterProvider.otherwise('home');
$stateProvider
.state('content',{
url: '/',
views:{
"":{templateUrl: 'partials/content.html'},
"header@content":{templateUrl: 'partials/header.html'},
}
})
.state('content.home',{
url: 'home',
views:{
"body@content":{templateUrl: 'partials/home.html'}
}
})
.state('content.photos',{
url: 'photos',
views:{
"body@content":{templateUrl: 'partials/photos.html'}
}
})
.state('content.about',{
url:'about',
views:{
"body@content":{templateUrl: 'partials/about.html'}
}
})
})

这时候，页面是这样呈现出来的：
→ 来到home这个路由

.state('content.home',{
url: 'home',
views:{
"body@content":{templateUrl: 'partials/home.html'}
}
})
以上，告诉我们partials/home.html将会被加载到与"body@content"匹配的ui-view中。暂时对应的ui-view还没有出现，于是等待。
路由看到index.html上的<div ui-view></div>
?
.state('content',{
url: '/',
views:{
"":{templateUrl: 'partials/content.html'},
"header@content":{templateUrl: 'partials/header.html'},
}
})
于是，就找到了content这个state下views下的 "":{templateUrl: 'partials/content.html'}这个键值对，把partials/content.html显示出来。
→ 分别加载partials/content.html页面上的各个部分
看到<div ui-view="header"></div>，就加载如下：
"header@content":{templateUrl: 'partials/header.html'},
看到<div ui-view="body"></div>，先加载 "body@content":{templateUrl: 'partials/home.html'}
→ 点击header上的链接
点击<a ui-sref="content.photos">Photos</a>，来到：
.state('content.photos',{
url: 'photos',
views:{
"body@content":{templateUrl: 'partials/photos.html'}
}
})
把partials/photos.html显示到<div ui-view="body"></div>中去。
点击<div ui-view="body"></div>，来到：
.state('content.about',{
url:'about',
views:{
"body@content":{templateUrl: 'partials/about.html'}
}
})
把partials/about.html显示到<div ui-view="body"></div>中去。

抽象state
如果一个state,没有通过链接找到它，那就可以把这个state设置为abstract:true,我们把以上的content和content.photos这2个state设置为抽象。
.state('content',{
url: '/',
abstract: true,
views:{
"":{templateUrl: 'partials/content.html'},
"header@content":{templateUrl: 'partials/header.html'},
}
})
...
.state('content.photos',{
url: 'photos',
abstract: true,
views:{
"body@content":{templateUrl: 'partials/photos.html'}
}
})
那么，当一个state设置为抽象，如果通过ui-sref或路由导航到该state会出现什么结果呢？
--会导航到默认路由上
$urlRouterProvider.otherwise('home');

.state('content.home',{
url: 'home',
views:{
"body@content":{templateUrl: 'partials/home.html'}
}
})

最终把partials/home.html显示出来。

http://www.jb51.net/article/78895.htm

可以设置一个abstract的父路由,这个控制器里面的数据是可以在他的子路由里面共享的,有些时候这么做还是蛮方便的.