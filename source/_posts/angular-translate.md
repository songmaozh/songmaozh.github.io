---
title: angular-translate
date: 2017-01-17 12:37:05
tags:
---

angular.js 国际化模块 angular-translate 简单方便快捷翻译中英文等多语言环境

很多web服务面对的不仅仅是当地用户,多语言环境不仅能提升逼格,更重要是一种用户体验.



angular.js 作为前后端拆分的解决方案之一,当然离不开前端框架处理国际化的问题,angular.js 官方出了一个模块 angular-translate 来解决多语言国际化问题.



我们前端采用 bower 包管理工具来管理依赖,点击链接查看bower 使用方法,这里不再详细说明.

上面列出的3个模块我们都要用到,一会详细说明:



今天和大家分享的内容目录:

使用angular-translate 模块的前期准备工作

创建过滤器做html页面内容的国际化

创建服务做javascript 脚本里的内容国际化





使用 angular-translate 模块的前期准备工作

使用 bower 管理工具下载 angular 及 angular-translate 模块
```python
bower install angular
bower install angular-translate
bower install angular-translate-loader-static-files
//然后在页面引用进去
< s cript src="/vender/angular-1.3.8.js"></ scrip t>
< script src="/vender/bower-angular-translate-2.4.2/angular-translate.min.js"></ scrip t>
< script src="/bower_components/angular-translate-loader-static-files/angular-translate-loader-static-files.min.js"></ scrip t>
第一个文件 angular-1.3.8.js 就不用多说了.你懂的.

第二个文件 angular-translate.min.js 是angular官方提供的国际化模块

第三个文件 angular-translate-loader-static-files.min.js 模块是用来读取本地文件的模块,因为我们的翻译内容都是独立的 json 文件.



我们找一个独立的文件夹 i18n 用来放json 文件,目录及文件如下层级关系:

/i18n/

        en.json

        cn.json



en.json 文件内容如下:

{"100001":"Login","100002":"Register"}
cn.json 文件内容如下:

{"100001":"登录","100002":"注册"}
上面2个json文件对应相同的键 ,我们称之为 翻译键.  不同的语言文件中,相同的翻译键对应相应的翻译值即可.如 "Login" 对应 "登录"





接下来我们需要在注入依赖:
```python
var app = angular.module('myApp', ['pascalprecht.translate'])
.config(['$translateProvider',function($translateProvider){
        var lang = window.localStorage.lang||'cn';
    $translateProvider.preferredLanguage(lang);
    $translateProvider.useStaticFilesLoader({
        prefix: '/i18n/',
        suffix: '.json'
    });
}]);
```
分解的看下上面的代码:

var app = angular.module('myApp', ['pascalprecht.translate']);
这一句就是告诉我们已经把 angular-translate 模块以一个依赖项加载进来.



.config(['$translateProvider',function($translateProvider)
config 函数用 $translateProvider 服务配置 $translate 服务实现.



我们上面使用了 localStorage.lang  来存储用户上一次选择的语言,如果用户是第一次范围,默认显示中文(及 加载 cn.json 文件来翻译)



$translateProvider.preferredLanguage(lang)
这一句告诉 angular.js 哪种语言是默认已注册的语言.



$translateProvider.useStaticFilesLoader({
        prefix: '/i18n/',
        suffix: '.json'
    });
上面的语句告诉我们 angular.js 应该加载本地那些国际化语言配置文件.

prefix : 指定文件前缀.

suffix: 指定文件后缀.



这时你可能会想,只有前缀和后缀那 它到底该加载那个文件呢.如果 i18n 里面有几十种语言翻译文件,是不是要全部加载?

不是这样的.它会按照默认注册的语言去加载.默认注册的语言就是下面这一句得到的.

var lang = window.localStorage.lang||'cn';
如果用户上次访问了英文站, window.localStorage.lang=en; 那么对于会加载 /i18n/en.json  文件

如果用户第一次访问, window.localStorage.lang=undefined ,那么默认我们会加载 /i18n/cn.json 文件



然后我们决定在页脚的位置放一个选择语言的下拉列表框


    <select class="language-switching" ng-controller="LanguageSwitchingCtrl" ng-model="cur_lang"        ng-change="switching(cur_lang)">
    <option value="en">English</option>
    <option value="cn">简体中文</option>
    </select>
上面的语言选择器提供了2种语言 ,en ,cn 

当我们选择项变化时会触发 ng-change 函数

空间还绑定了模型 ng-model="cur_lang"



然后我们看下 controller 里面的内容:

angular.module('myApp').controller('LanguageSwitchingCtrl', ['$scope', '$translate', function (scope, $translate) {
    scope.switching = function(lang){
        $translate.use(lang);
        window.localStorage.lang = lang;
        window.location.reload();
    };
    scope.cur_lang = $translate.use();
}]);
ng-change 事件触发时会执行 控制器的 switching 方法. 此方法会接受下拉列表 option 传过来的参数值 (en 或者 cn )

然后执行 $translate.use(lang) 方法.此方法实现了在运行时切换语言的功能.



那 ng-model 到底实现了什么功能呢?这里的作用就是页面加载时下拉列表显示出当前默认使用的是哪种语言,就是定位select 默认项.



我们所有的准备环境都配置好了,下面开始介绍应用:



2.创建过滤器做Html 页面内容的国际化

现在我们先准备在html页面里做国际化,首先想到做一个过滤器,在html页面使用起来是最方便的. /filters/ 目录下创建 T.js 过滤器

angular.module("myApp").filter("T", ['$translate', function($translate) {
    return function(key) {
        if(key){
            return $translate.instant(key);
        }
    };
}]);


过滤器使用起来非常简单方便,加入我们要在一个登录页面里,登录和注册链接需要我们做国际化.

‍

<div ng-controller="LoginCtrl" >
    

         

         <a ui-sref="app.login({})">{{'100001'|T}}
         <a ui-sref="app.register({})">{{'100002'|T}}
         


    

这样在不同的语言环境, angular.js 会加载不同的语言配置文件,根据翻译ID展示出来翻译值.



3.我们在javascript 脚本中使用国际化

当然有人说直接用过滤器来做,也是可以的,但是个人更喜欢创建一个服务,感觉使用起来简单方法

我们在 /services/ 目录里创建 T.js 服务,内容如下:
```python

angular.module('myApp').factory('T', ['$translate', function($translate) {
    var T = {
        T:function(key) {
            if(key){
                return $translate.instant(key);
            }
            return key;
        }
    }
    return T;
}]);
```
 服务T 返回了一个方法 T.下面我们样式一下如何在controller 里使用国际化.



假如登录的控制器 LoginCtrl.js 有一个登录标签需要做国际化:

angular.module('myApp').controller('LgoinCtrl', ['$scope','T',
    function($scope,T) {
        
        $scope.login_title=T.T(100001);
    
    }
]);
首先需要把 T 服务依赖注入到控制器,然后在需要国际化的地方直接调用 T 服务的 T 方法,传入翻译ID 返回 翻译值.

走进AngularJs(七) 过滤器（filter）

AngularJS模块:可以在被加载和执行之前对自身进行配置 我们可以在应用加载阶段配置不同的逻辑  
    ##配置快:  
        通过config方法实现对模块的配置,AngularJS中的服务多数都对应一个provider,  
        用来执行与对应服务相同的功能或对其配置,比如$log、$http、$location都是内置服务，  
        相对应的“provider”分别是$logProvider、$httpProvider、$locationPorvider。  
    ##运行块:  
        服务也是模块形式存在的对且对外提供特定功能，前面学习中都是将服务做为依赖注入进去的，  
            然后再进行调用，除了这种方式外我们也可以直接运行相应的服务模块，  
            AngularJS提供了run方法来实现。  
        run方法还是最先执行的，利用这个特点我们可以将一些需要优先执行的功能通过run方法来运行，  
            比如验证用户是否登录，未登录则不允许进行任何其它操作。  

配置块会按照$provide, $compileProvider, $filterProvider，注册的顺序，依次被应用。唯一的例外是对常量的定义，它们应该始终放在配置块的开始处。
运行块是AngularJS中最像主方法的东西。一个运行块就是一段用来启动应用的代码。它在所有服务都被配置和所有的注入器都被创建后执行。运行块通常包含了一些难以测试的代码，所以它们应该写在单独的模块里，这样在单元测试时就可以忽略它们了。
模块可以把其他模块列为它的依赖。“依赖某个模块”意味着需要把这个被依赖的模块在本块模块之前被加载。换句话说被依赖模块的配置块会在本模块配置块前被执行。运行块也是一样。任何一个模块都只能被加载一次，即使它被多个模块依赖。
模块是一种用来管理$injector配置的方法，和脚本的加载没有关系。现在网上已有很多控制模块加载的库，它们可以和AngularJS配合使用。因为在加载期间模块不做任何事情，所以它们可以以任意顺序或者并行方式加载



http://harttle.com/2015/06/07/angular-module.html
