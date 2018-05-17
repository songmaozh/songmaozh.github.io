---
title: AngularJS ocLoayLoad
date: 2017-02-14 10:05:23
tags:
---

初学者，有不足的地方希望各位指出

　　一、前言

　　　　ocLoayLoad是AngularJS的模块按需加载器。一般在小型项目里，首次加载页面就下载好所有的资源没有什么大问题。但是当我们的网站渐渐庞大起来，这样子的加载策略让网速初始化速度变得越来越慢，用户体验不好。二来，分模块加载易于团队协作，减低代码冲突。

　　二、按需加载的对象

　　　　各个Controller模块、Directive模块、Server模块、template模板，其实这些都是一些 .js文件或者 .html文件 。

　   三 、按需加载的场景

　　　　三、1 路由加载(resolve/uiRouter)

　　　　　　基于uiRouter的resolve是在加载controller和template之前所执行的一系列操作，它帮助我们初始化我们所要前往的那一个视图。只有be solved(操作完成)，controller才会被实例化。因此，我们可以在resolve步骤里面加载我们所需要的controller。

复制代码
    $stateProvider
        .state('index', {
            url: '/',
            views: {
                'lazyLoadView': {
                    templateUrl: 'partials/main.html',
                    controller: 'AppCtrl'
                }    
            },
            resolve: {
                loadMyCtrl: ['$ocLazyLoad', function($ocLazyLoad){
                    return $ocLazyLoad.load('js/AppCtrl.js')
                }]
            }
        })
复制代码
　　　　其中，'js/AppCtrl.js'里面放着一个我们所需要的controller

angular.module('myApp')
.controller('AppCtrl', ['$scope', function($scope){
//...
}])
　　　　 三、2 依赖加载

　　　　　　在依赖项里面导入我们所需要的一系列模块(这里有一层'[ ]'符合哦)

复制代码
angular.module('gridModule', [[
    'bower_components/angular-ui-grid/ui-grid.js',
    'bower_components/angular-ui-grid/ui-grid.css'
]]).controller('GridModuleCtrl', ['$scope', function($scope){
    //...
}])
复制代码
　　　　 三、3 Cotroller里动态加载

复制代码
angular.module('myApp')
.controller('AppCtrl', ['$scope','$ocLazyLoad', function($scope, $ocLazyLoad){
    $scope.loadBootstrap = function(){
        $ocLazyLoad.load([
          'bower_components/bootstrap/dist/js/bootstrap.js',
          'bower_components/bootstrap/dist/css/bootstrap.css'
        ])
    }
   var unbind = $scope.$on('ocLazyLoad.fileLoaded', function(e, file){
        $scope.bootstrapLoaded = true;
         console.log('下载boot完成');
         unbind();
    })
}])
复制代码
　　　　 三、4 template包含加载(config)

　　　　如何处理我们所加载的html模板里面嵌套的controller呢？这里需要oc-lazy-load指令和$ocLazyLoadProvider的配置

复制代码
/*template A.html*/
<h1>hi i am hzp </h1>
    <div oc-lazy-load="gridModule">
        <div ng-controller="GridModuleCtrl">
            <span>{{test}}</span><br/>
            <div ui-grid="gridOptions" class="gridStyle"></div>
        </div>
    </div>
</div>
复制代码
 

复制代码
    $ocLazyLoadProvider.config({
        modules: [{
            name: 'gridModule',
            files: [
                'js/gridModule.js'
            ]
        }]
    })
复制代码
　　　　四、如何组织按需加载

　　　　分路由、按功能来区分、打包成不同的多个或单个controller.directive.server模块

 

 最近项目比较忙额，白天要上班，晚上回来还需要做Angular知识点的ppt给同事，毕竟年底要辞职了，项目的后续开发还是需要有人接手的，所以就占用了晚上学习的时间。本来一直不打算写这些第三方的学习笔记，不过觉得按需加载模块并且成功使用这个确实是个好处，还是记录下来吧。基于本兽没怎么深入的使用requireJs，所以本兽不知道这个和requireJs有什么区别，也不能清晰的说明这到底算不算Angular的按需加载。

为了实现这篇学习笔记知识点的效果，我们需要用到：

angular: https://github.com/angular/angular.js

ui-router: https://github.com/angular-ui/ui-router

ocLazyLoad: https://github.com/ocombe/ocLazyLoad

废话不多说，进入正题...

首先我们看下 文件结构：

Angular-ocLazyLoad                      --- demo文件夹
    Scripts                             --- 框架及插件文件夹
        angular-1.4.7                   --- angular 不解释
        angular-ui-router               --- uirouter 不解释
        oclazyload                      --- ocLazyload 不解释
        bootstrap                       --- bootstrap 不解释
        angular-tree-control-master     --- angular-tree-control-master 不解释
        ng-table                        --- ng-table 不解释
        angular-bootstrap               --- angular-bootstrap 不解释
    js                                  --- js文件夹 针对demo写的js文件
        controllers                     --- 页面控制器文件夹
            angular-tree-control.js     --- angular-tree-control控制器代码
            default.js                  --- default控制器代码
            ng-table.js                 --- ng-table控制器代码
        app.config.js                   --- 模块注册及配置代码
        oclazyload.config.js            --- 加载模块配置代码
        route.config.js                 --- 路由配置及加载代码
    views                               --- html页面文件夹
        angular-tree-control.html       --- angular-tree-control插件的效果页面
        default.html                    --- default页面
        ng-table.html                   --- ng-table插件效果页面
        ui-bootstrap.html               --- uibootstrap插件效果页面
    index.html                          --- 主页面
注意：这个demo没做嵌套路由，只是简单实现单视图的路由以展示效果。

我们来看主页面的代码：

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title></title>
    <link rel="stylesheet" href="Scripts/bootstrap/dist/css/bootstrap.min.css" />
    <script src="Scripts/angular-1.4.7/angular.js"></script>
    <script src="Scripts/angular-ui-router/release/angular-ui-router.min.js"></script>
    <script src="Scripts/oclazyload/dist/ocLazyLoad.min.js"></script>
    <script src="js/app.config.js"></script>
    <script src="js/oclazyload.config.js"></script>
    <script src="js/route.config.js"></script>
</head>
<body>
<div ng-app="templateApp">
    <div>
        <a href="#/default">主页</a>
        <a href="#/uibootstrap" >ui-bootstrap</a>
        <a href="#/ngtable">ng-table</a>
        <a href="#/ngtree">angular-tree-control</a>
    </div>
    <div ui-view></div>
</div>
</body>
</html>
在这个页面，我们只加载了bootstrap的css、angular的js、ui-router的js、ocLazyLoad的js，以及3个配置的js文件。

再看看四个页面的html代码：

angular-tree-control效果页面

<treecontrol tree-model="ngtree.treeData" class="tree-classic ng-cloak" options="ngtree.treeOptions">
      {{node.title}}
  </treecontrol>
页面上有个使用该插件对应的指令。

default页面

<div class="ng-cloak">
      {{default.value}}
  </div>
这里我们只是用来证明加载并正确执行default.js。

ng-table效果页面

<div class="ng-cloak">
    <div class="p-h-md p-v bg-white box-shadow pos-rlt">
        <h3 class="no-margin">ng-table</h3>
    </div>
    <button ng-click="ngtable.tableParams.sorting({})" class="btn btn-default pull-right">Clear sorting</button>
    <p>
        <strong>Sorting:</strong> {{ngtable.tableParams.sorting()|json}}
    </p>
    <table ng-table="ngtable.tableParams" class="table table-bordered table-striped">
        <tr ng-repeat="user in $data">
            <td data-title="'Name'" sortable="'name'">
                {{user.name}}
            </td>
            <td data-title="'Age'" sortable="'age'">
                {{user.age}}
            </td>
        </tr>
    </table>
</div>
这里写了些简单的ng-table效果。

ui-bootstrap效果页面

<span uib-dropdown class="ng-cloak">
      <a href id="simple-dropdown" uib-dropdown-toggle>
          下拉框触发
      </a>
      <ul class="uib-dropdown-menu dropdown-menu" aria-labelledby="simple-dropdown">
          下拉框内容.这里写个效果证明实现动态加载即可
      </ul>
  </span>
这里仅写了个下拉框效果，证明正确加载并使用该插件。

好了，看完了html，我们看下 加载配置和路由配置 ：

"use strict"
var tempApp = angular.module("templateApp",["ui.router","oc.lazyLoad"])
.config(["$provide","$compileProvider","$controllerProvider","$filterProvider",
                function($provide,$compileProvider,$controllerProvider,$filterProvider){
                    tempApp.controller = $controllerProvider.register;
                    tempApp.directive = $compileProvider.register;
                    tempApp.filter = $filterProvider.register;
                    tempApp.factory = $provide.factory;
                    tempApp.service  =$provide.service;
                    tempApp.constant = $provide.constant;
                }]);
以上代码对模块的注册，仅仅依赖了ui.router和oc.LazyLoad。配置也只是简单配置了模块，以便在后面的js能识别到tempApp上的方法。

然后我们再看看 ocLazyLoad加载模块的配置 ：

"use strict"
tempApp
.constant("Modules_Config",[
    {
        name:"ngTable",
        module:true,
        files:[
            "Scripts/ng-table/dist/ng-table.min.css",
            "Scripts/ng-table/dist/ng-table.min.js"
        ]
    },
    {
        name:"ui.bootstrap",
        module:true,
        files:[
            "Scripts/angular-bootstrap/ui-bootstrap-tpls-0.14.3.min.js"
        ]
    },
    {
        name:"treeControl",
        module:true,
        files:[
            "Scripts/angular-tree-control-master/css/tree-control.css",
            "Scripts/angular-tree-control-master/css/tree-control-attribute.css",
            "Scripts/angular-tree-control-master/angular-tree-control.js"
        ]
    }
])
.config(["$ocLazyLoadProvider","Modules_Config",routeFn]);
function routeFn($ocLazyLoadProvider,Modules_Config){
    $ocLazyLoadProvider.config({
        debug:false,
        events:false,
        modules:Modules_Config
    });
};
路由的配置：

"use strict"
tempApp.config(["$stateProvider","$urlRouterProvider","$locationProvider",routeFn]);
function routeFn($stateProvider,$urlRouterProvider,$locationProvider){
    $urlRouterProvider.otherwise("/default");
    $stateProvider
    .state("default",{
        url:"/default",
        views:{
            "":{
                templateUrl:"views/default.html",
                controller:"defaultCtrl",
                controllerAs:"default"
            }
        },
        resolve:{
            deps:["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("js/controllers/default.js");
            }]
        } 
    })
    .state("uibootstrap",{
        url:"/uibootstrap",
        views:{
            "":{
                templateUrl:"views/ui-bootstrap.html"
            }
        },
        resolve:{
            deps:["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("ui.bootstrap");
            }]
        } 
    })
    .state("ngtable",{
        url:"/ngtable",
        views:{
            "":{
                templateUrl:"views/ng-table.html",
                controller:"ngTableCtrl",
                controllerAs:"ngtable"
            }
        },
        resolve:{
            deps:["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("ngTable").then(
                    function(){
                        return $ocLazyLoad.load("js/controllers/ng-table.js");
                    }
                );
            }]
        } 
    })
    .state("ngtree",{
        url:"/ngtree",
        views:{
            "":{
                templateUrl:"views/angular-tree-control.html",
                controller:"ngTreeCtrl",
                controllerAs:"ngtree"
            }
        },
        resolve:{
            deps:["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("treeControl").then(
                    function(){
                        return $ocLazyLoad.load("js/controllers/angular-tree-control.js");
                    }
                );
            }]
        } 
    })
};
ui-bootstrap的下拉框简单的实现不需要控制器，那么我们就只看看ng-table和angular-tree-control的控制器js吧：

ng-table.js

(function(){
"use strict"
tempApp
.controller("ngTableCtrl",["$location","NgTableParams","$filter",ngTableCtrlFn]);
function ngTableCtrlFn($location,NgTableParams,$filter){
    var vm = this;
    //数据
    var data = [{ name: "Moroni", age: 50 },
                 { name: "Tiancum ", age: 43 },
                 { name: "Jacob", age: 27 },
                 { name: "Nephi", age: 29 },
                 { name: "Enos", age: 34 },
                 { name: "Tiancum", age: 43 },
                 { name: "Jacob", age: 27 },
                 { name: "Nephi", age: 29 },
                 { name: "Enos", age: 34 },
                 { name: "Tiancum", age: 43 },
                 { name: "Jacob", age: 27 },
                 { name: "Nephi", age: 29 },
                 { name: "Enos", age: 34 },
                 { name: "Tiancum", age: 43 },
                 { name: "Jacob", age: 27 },
                 { name: "Nephi", age: 29 },
                 { name: "Enos", age: 34 }];
    vm.tableParams = new NgTableParams(    // 合并默认的配置和url参数
        angular.extend({
            page: 1,            // 第一页
            count: 10,          // 每页的数据量
           sorting: {
                name: 'asc'     // 默认排序
            }
        },
        $location.search())
        ,{
            total: data.length, // 数据总数
            getData: function ($defer, params) {
                $location.search(params.url()); // 将参数放到url上，实现刷新页面不会跳回第一页和默认配置
                var orderedData = params.sorting ?
                        $filter('orderBy')(data, params.orderBy()) :data;
                $defer.resolve(orderedData.slice((params.page() - 1) * params.count(), params.page() * params.count()));
            }
        }
    );
};
})();
angular-tree-control.js

(function(){
"use strict"
tempApp
.controller("ngTreeCtrl",ngTreeCtrlFn);
function ngTreeCtrlFn(){
    var vm = this;
    //树数据
    vm.treeData = [
                {
                    id:"1",
                    title:"标签1",
                    childList:[
                        {
                            id:"1-1",
                            title:"子级1",
                            childList:[
                                {
                                    id:"1-1-1",
                                    title:"再子级1",
                                    childList:[]
                                }
                            ]
                        },
                        {
                            id:"1-2",
                            title:"子级2",
                            childList:[
                                {
                                    id:"1-2-1",
                                    title:"再子级2",
                                    childList:[
                                        {
                                            id:"1-2-1-1",
                                            title:"再再子级1",
                                            childList:[]
                                        }
                                    ]
                                }
                            ]
                        },
                        {
                            id:"1-3",
                            title:"子级3",
                            childList:[]
                        }
                    ]
                },
                {
                    id:"2",
                    title:"标签2",
                    childList:[
                        {
                            id:"2-1",
                            title:"子级1",
                            childList:[]
                        },
                        {
                            id:"2-2",
                            title:"子级2",
                            childList:[]
                        },
                        {
                            id:"2-3",
                            title:"子级3",
                            childList:[]
                        }
                    ]}
                ,
                {
                    id:"3",
                    title:"标签3",
                    childList:[
                        {
                            id:"3-1",
                            title:"子级1",
                            childList:[]
                        },
                        {
                            id:"3-2",
                            title:"子级2",
                            childList:[]
                        },
                        {
                            id:"3-3",
                            title:"子级3",
                            childList:[]
                        }
                    ]
                }
            ];
    //树配置
    vm.treeOptions = {
      nodeChildren:"childList",
        dirSelectable:false
    };
};
})();


https://oclazyload.readme.io/