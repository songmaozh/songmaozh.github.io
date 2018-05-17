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