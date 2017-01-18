---
title: AngularJS
date: 2017-01-12 11:01:24
tags:
---

angular -  ng关键字缩写
AngularJS 是一个 JavaScript 框架。它可通过 script 标签添加到 HTML 页面。
AngularJS 通过 指令 扩展了 HTML，且通过 表达式 绑定数据到 HTML。
AngularJS 是一个 JavaScript 框架
AngularJS 是一个 JavaScript 框架。它是一个以 JavaScript 编写的库。
AngularJS 是以一个 JavaScript 文件形式发布的，可通过 script 标签添加到网页中：

```python
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js">
</script>
```

各个 angular.js 版本下载： https://github.com/angular/angular.js/releases
AngularJS 扩展了 HTML
AngularJS 通过 ng-directives 扩展了 HTML。
ng-app 指令定义一个 AngularJS 应用程序。
ng-model 指令把元素值（比如输入域的值）绑定到应用程序。
ng-bind 指令把应用程序数据绑定到 HTML 视图。
AngularJS 实例

```python
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></scrip t >
</head>
<body>
 
<div ng-app="">
     <p>名字 : <input type="text" ng-model="name"></p>
     <h1>Hello {{name}}</h1>
</div>
 
</body>
</html>
```

实例讲解：
当网页加载完毕，AngularJS 自动开启。
ng-app 指令告诉 AngularJS，<div> 元素是 AngularJS 应用程序 的"所有者"。
ng-model 指令把输入域的值绑定到应用程序变量 name。
ng-bind 指令把应用程序变量 name 绑定到某个段落的 innerHTML。

什么是 AngularJS？
AngularJS 使得开发现代的单一页面应用程序（SPAs：Single Page Applications）变得更加容易。
AngularJS 把应用程序数据绑定到 HTML 元素。
AngularJS 可以克隆和重复 HTML 元素。
AngularJS 可以隐藏和显示 HTML 元素。
AngularJS 可以在 HTML 元素"背后"添加代码。
AngularJS 支持输入验证。
AngularJS 指令
正如您所看到的，AngularJS 指令是以 ng 作为前缀的 HTML 属性。
ng-init 指令初始化 AngularJS 应用程序变量。
AngularJS 实例
```python
<div ng-app="" ng-init="firstName='John'">
 
<p>姓名为 <span ng-bind="firstName"></span></p>
 
</div>
```
HTML5 允许扩展的（自制的）属性，以 data- 开头。
AngularJS 属性以 ng- 开头，但是您可以使用 data-ng- 来让网页对 HTML5 有效。
AngularJS 实例
```python
<div data-ng-app="" data-ng-init="firstName='John'">
 
<p>姓名为 <span data-ng-bind="firstName"></span></p>
 
</div>
```
AngularJS 表达式
AngularJS 表达式写在双大括号内：{{ expression }}。
AngularJS 表达式把数据绑定到 HTML，这与 ng-bind 指令有异曲同工之妙。
AngularJS 将在表达式书写的位置"输出"数据。
AngularJS 表达式 很像 JavaScript 表达式：它们可以包含文字、运算符和变量。
实例 {{ 5 + 5 }} 或 {{ firstName + " " + lastName }}
AngularJS 实例
```python
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></scrip t> 
</head>
<body>
 
<div ng-app="">
     <p>我的第一个表达式： {{ 5 + 5 }}</p>
</div>
 
</body>
</html>
```
AngularJS 应用
AngularJS 模块（Module） 定义了 AngularJS 应用。
AngularJS 控制器（Controller） 用于控制 AngularJS 应用。
ng-app指令定义了应用, ng-controller 定义了控制器。
AngularJS 实例
```python
<div ng-app="myApp" ng-controller="myCtrl">
 
名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}
 
</div>
 
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName= "John";
    $scope.lastName= "Doe";
});
</scrip t>
```
AngularJS 模块定义应用:
AngularJS 模块
```python
var app = angular.module('myApp', []);
```
AngularJS 控制器控制应用:
AngularJS 控制器
```python
app.controller('myCtrl', function($scope) {
    $scope.firstName= "John";
    $scope.lastName= "Doe";
});
```
使用 ng-init 不是很常见。您将在控制器一章中学习到一个更好的初始化数据的方式。
## 表达式
```python
数字
<div ng-app="" ng-init="quantity=1;cost=5">
 
<p>总价： {{ quantity * cost }}</p>
 
</div>

字符串
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
 
<p>姓名： {{ firstName + " " + lastName }}</p>
 
</div>

对象
<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">
 
<p>姓为 {{ person.lastName }}</p>
 
</div>

数组
<div ng-app="" ng-init="points=[1,15,19,2,40]">
 
<p>第三个值为 {{ points[2] }}</p>
 
</div>
```
AngularJS 表达式 与 JavaScript 表达式
类似于 JavaScript 表达式，AngularJS 表达式可以包含字母，操作符，变量。
与 JavaScript 表达式不同，AngularJS 表达式可以写在 HTML 中。
与 JavaScript 表达式不同，AngularJS 表达式不支持条件判断，循环及异常。
与 JavaScript 表达式不同，AngularJS 表达式支持过滤器。

## AngularJS 指令
AngularJS 通过被称为 指令 的新属性来扩展 HTML。
AngularJS 通过内置的指令来为应用添加功能。
AngularJS 允许你自定义指令。
AngularJS 指令是扩展的 HTML 属性，带有前缀 ng-。
ng-app 指令初始化一个 AngularJS 应用程序。
ng-init 指令初始化应用程序数据。
ng-model 指令把元素值（比如输入域的值）绑定到应用程序。

ng-app 指令告诉 AngularJS，<div> 元素是 AngularJS 应用程序 的"所有者"。
一个网页可以包含多个运行在不同元素中的 AngularJS 应用程序。

## 数据绑定
上面实例中的 {{ firstName }} 表达式是一个 AngularJS 数据绑定表达式。
AngularJS 中的数据绑定，同步了 AngularJS 表达式与 AngularJS 数据。
{{ firstName }} 是通过 ng-model="firstName" 进行同步。
在下一个实例中，两个文本域是通过两个 ng-model 指令同步的：
```python
<div ng-app="" ng-init="quantity=1;price=5">
 
<h2>价格计算器</h2>
 
数量： <input type="number"    ng-model="quantity">
价格： <input type="number" ng-model="price">
 
<p><b>总价：</b> {{ quantity * price }}</p>
 
</div>
```

## 重复 HTML 元素
ng-repeat 指令会重复一个 HTML 元素：
AngularJS 实例
```python
<div ng-app="" ng-init="names=['Jani','Hege','Kai']">
  <p>使用 ng-repeat 来循环数组</p>
  <ul>
    <li ng-repeat="x in names">
      {{ x }}
    </li>
  </ul>
</div>
```
ng-repeat 指令用在一个对象数组上：
AngularJS 实例
```python
<div ng-app="" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">
 
<p>循环对象：</p>
<ul>
  <li ng-repeat="x    in names">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
 
</div>
```
AngularJS 完美支持数据库的 CRUD（增加Create、读取Read、更新Update、删除Delete）应用程序。
把实例中的对象想象成数据库中的记录。

ng-app 指令
ng-app 指令定义了 AngularJS 应用程序的 根元素。
ng-app 指令在网页加载完毕时会自动引导（自动初始化）应用程序。
稍后您将学习到 ng-app 如何通过一个值（比如 ng-app="myModule"）连接到代码模块。
ng-init 指令
ng-init 指令为 AngularJS 应用程序定义了 初始值。
通常情况下，不使用 ng-init。您将使用一个控制器或模块来代替它。
稍后您将学习更多有关控制器和模块的知识。
ng-model 指令
ng-model 指令 绑定 HTML 元素 到应用程序数据。
ng-model 指令也可以：
为应用程序数据提供类型验证（number、email、required）。
为应用程序数据提供状态（invalid、dirty、touched、error）。
为 HTML 元素提供 CSS 类。
绑定 HTML 元素到 HTML 表单。
ng-repeat 指令
ng-repeat 指令对于集合中（数组中）的每个项会 克隆一次 HTML 元素。

创建自定义的指令
除了 AngularJS 内置的指令外，我们还可以创建自定义指令。
你可以使用 .directive 函数来添加自定义的指令。
要调用自定义指令，HTML 元素上需要添加自定义指令名。
使用驼峰法来命名一个指令， runoobDirective, 但在使用它时需要以 - 分割, runoob-directive:
AngularJS 实例
```python
<body ng-app="myApp">

<runoob-directive></runoob-directive>

<script>
var app = angular.module("myApp", []);
app.directive("runoobDirective", function() {
    return {
        template : "<h1>自定义指令!</h1>"
    };
});
</scrip t>

</body>
```
你可以通过以下方式来调用指令：
元素名
属性
类名
注释
以下实例方式也能输出同样结果:

```python
元素名
<runoob-directive></runoob-directive>

属性
<div runoob-directive></div>

类名
<div class="runoob-directive"></div>

注释
<!-- directive: runoob-directive -->

```
限制使用
你可以限制你的指令只能通过特定的方式来调用。
实例
通过添加 restrict 属性,并设置只值为 "A", 来设置指令只能通过属性的方式来调用:
```python
var app = angular.module("myApp", []);
app.directive("runoobDirective", function() {
    return {
        restrict : "A",
        template : "<h1>自定义指令!</h1>"
    };
});
```
restrict 值可以是以下几种:
E 作为元素名使用
A 作为属性使用
C 作为类名使用
M 作为注释使用
restrict 默认值为 EA, 即可以通过元素名和属性名来调用指令。

```python
angular自定义指令的两种写法：
上面这种，感觉更清晰明确一点。
// angular.module('MyApp',[])
// .directive('zl1',zl1)
// .controller('con1',['$scope',func1]);
//
// function zl1(){
//   var directive={
//     restrict:'AEC',
//     template:'this is the it-first directive',
//   };
//   return directive;
// };
//
// function func1($scope){
//   $scope.name="alice";
// }

//这是教程里类似的写法
angular.module('myApp',[]).directive('zl1',[ function(){
  return {
    restrict:'AE',
    template:'thirective',
    link:function($scope,elm,attr,controller){
      console.log("这是link");
    },
    controller:function($scope,$element,$attrs){
      console.log("这是con");
    }
  };
}]).controller('Con1',['$scope',function($scope){
  $scope.name="aliceqqq";
}]);
Alice2周前 (12-29)

还有指令配置项的：link controller等在项目运用中有遇到过：
angular.module('myApp', []).directive('first', [ function(){
    return {
        // scope: false, // 默认值，共享父级作用域
        // controller: function($scope, $element, $attrs, $transclude) {},
        restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
        template: 'first name:{{name}}',
    };
}]).directive('second', [ function(){
    return {
        scope: true, // 继承父级作用域并创建指令自己的作用域
        // controller: function($scope, $element, $attrs, $transclude) {},
        restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
        //当修改这里的name时，second会在自己的作用域中新建一个name变量，与父级作用域中的
        // name相对独立，所以再修改父级中的name对second中的name就不会有影响了
        template: 'second name:{{name}}',
    };
}]).directive('third', [ function(){
    return {
        scope: {}, // 创建指令自己的独立作用域，与父级毫无关系
        // controller: function($scope, $element, $attrs, $transclude) {},
        restrict: 'AE', // E = Element, A = Attribute, C = Class, M = Comment
        template: 'third name:{{name}}',
    };
}])
.controller('DirectiveController', ['$scope', function($scope){
    $scope.name="mike";
}]);
```

## AngularJS ng-model 指令
ng-model 指令用于绑定应用程序数据到 HTML 控制器(input, select, textarea)的值。
ng-model 指令
ng-model 指令可以将输入域的值与 AngularJS 创建的变量绑定。
AngularJS 实例
```python
<div ng-app="myApp" ng-controller="myCtrl">
    名字: <input ng-model="name">
</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.name = "John Doe";
});
</scrip t>
```

双向绑定
双向绑定，在修改输入域的值时， AngularJS 属性的值也将修改：
AngularJS 实例
```python
<div ng-app="myApp" ng-controller="myCtrl">
    名字: <input ng-model="name">
    <h1>你输入了: {{name}}</h1>
</div>
```

验证用户输入
AngularJS 实例
```python
<form ng-app="" name="myForm">
    Email:
    <input type="email" name="myAddress" ng-model="text">
    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
</form>
```
以上实例中，提示信息会在 ng-show 属性返回 true 的情况下显示。

应用状态
ng-model 指令可以为应用数据提供状态值(invalid, dirty, touched, error):
AngularJS 实例
```python
<form ng-app="" name="myForm" ng-init="myText = 'test@runoob.com'">
    Email:
    <input type="email" name="myAddress" ng-model="myText" required></p>
    <h1>状态</h1>
    {{myForm.myAddress.$valid}}
    {{myForm.myAddress.$dirty}}
    {{myForm.myAddress.$touched}}
</form>
```

CSS 类
ng-model 指令基于它们的状态为 HTML 元素提供了 CSS 类：
AngularJS 实例
```python
<style>
input.ng-invalid {
    background-color: lightblue;
}
</style>
<body>

<form ng-app="" name="myForm">
    输入你的名字:
    <input name="myAddress" ng-model="text" required>
</form>
```
ng-model 指令根据表单域的状态添加/移除以下类：
ng-empty
ng-not-empty
ng-touched
ng-untouched
ng-valid
ng-invalid
ng-dirty
ng-pending
ng-pristine

## AngularJS 控制器
AngularJS 控制器 控制 AngularJS 应用程序的数据。
 AngularJS 控制器是常规的 JavaScript 对象。
 AngularJS 控制器
AngularJS 应用程序被控制器控制。
ng-controller 指令定义了应用程序控制器。
控制器是 JavaScript 对象，由标准的 JavaScript 对象的构造函数 创建。
AngularJS 实例
```python
<div ng-app="myApp" ng-controller="myCtrl">

名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{firstName + " " + lastName}}

</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
</scrip t >
```
应用解析：
AngularJS 应用程序由 ng-app 定义。应用程序在 <div> 内运行。
ng-controller="myCtrl" 属性是一个 AngularJS 指令。用于定义一个控制器。
myCtrl 函数是一个 JavaScript 函数。
AngularJS 使用$scope 对象来调用控制器。
在 AngularJS 中， $scope 是一个应用对象(属于应用变量和函数)。
控制器的 $scope （相当于作用域、控制范围）用来保存AngularJS Model(模型)的对象。
控制器在作用域中创建了两个属性 (firstName 和 lastName)。
ng-model 指令绑定输入域到控制器的属性（firstName 和 lastName）。

控制器方法
上面的实例演示了一个带有 lastName 和 firstName 这两个属性的控制器对象。
控制器也可以有方法（变量和函数）：
AngularJS 实例
```python
<div ng-app="myApp" ng-controller="personCtrl">

名: <input type="text" ng-model="firstName"><br>
姓: <input type="text" ng-model="lastName"><br>
<br>
姓名: {{fullName()}}

</div>

<script>
var app = angular.module('myApp', []);
app.controller('personCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
    $scope.fullName = function() {
        return $scope.firstName + " " + $scope.lastName;
    }
});
</scrip t>
```

## AngularJS 包含
在 AngularJS 中，你可以在 HTML 中包含 HTML 文件。
使用 AngularJS, 你可以使用 ng-include 指令来包含 HTML 内容:
实例
```python
<body ng-app="">
 
<div ng-include="'runoob.htm'"></div>
 
</body>
```

## AngularJS 依赖注入
依赖注入（Dependency Injection，简称DI）是一种软件设计模式，在这种模式下，一个或更多的依赖（或服务）被注入（或者通过引用传递）到一个独立的对象（或客户端）中，然后成为了该客户端状态的一部分。
该模式分离了客户端依赖本身行为的创建，这使得程序设计变得松耦合，并遵循了依赖反转和单一职责原则。与服务定位器模式形成直接对比的是，它允许客户端了解客户端如何使用该系统找到依赖
AngularJS 提供很好的依赖注入机制。以下5个核心组件用来作为依赖注入：
value
factory
service
provider
constant

value
Value 是一个简单的 javascript 对象，用于向控制器传递值（配置阶段）：
```python
// 定义一个模块
var mainApp = angular.module("mainApp", []);

// 创建 value 对象 "defaultInput" 并传递数据
mainApp.value("defaultInput", 5);
...

// 将 "defaultInput" 注入到控制器
mainApp.controller('CalcController', function($scope, CalcService, defaultInput) {
   $scope.number = defaultInput;
   $scope.result = CalcService.square($scope.number);
   
   $scope.square = function() {
      $scope.result = CalcService.square($scope.number);
   }
});
```
factory
factory 是一个函数用于返回值。在 service 和 controller 需要时创建。
通常我们使用 factory 函数来计算或返回值。
```python
// 定义一个模块
var mainApp = angular.module("mainApp", []);

// 创建 factory "MathService" 用于两数的乘积 provides a method multiply to return multiplication of two numbers
mainApp.factory('MathService', function() {
   var factory = {};
   
   factory.multiply = function(a, b) {
      return a * b
   }
   return factory;
}); 

// 在 service 中注入 factory "MathService"
mainApp.service('CalcService', function(MathService){
   this.square = function(a) {
      return MathService.multiply(a,a);
   }
});
...
```
provider
AngularJS 中通过 provider 创建一个 service、factory等(配置阶段)。
Provider 中提供了一个 factory 方法 get()，它用于返回 value/service/factory。
```python
// 定义一个模块
var mainApp = angular.module("mainApp", []);
...

// 使用 provider 创建 service 定义一个方法用于计算两数乘积
mainApp.config(function($provide) {
   $provide.provider('MathService', function() {
      this.$get = function() {
         var factory = {};  
         
         factory.multiply = function(a, b) {
            return a * b; 
         }
         return factory;
      };
   });
});
```

constant
constant(常量)用来在配置阶段传递数值，注意这个常量在配置阶段是不可用的。
mainApp.constant("configParam", "constant value");

以下实例提供了以上几个依赖注入机制的演示。
```python
<html>
   
   <head>
      <meta charset="utf-8">
      <title>AngularJS  依赖注入</title>
   </head>
   
   <body>
      <h2>AngularJS 简单应用</h2>
      
      <div ng-app = "mainApp" ng-controller = "CalcController">
         <p>输入一个数字: <input type = "number" ng-model = "number" /></p>
         <button ng-click = "square()">X<sup>2</sup></button>
         <p>结果: {{result}}</p>
      </div>
      
      <script src="http://apps.bdimg.com/libs/angular.js/1.4.6/angular.min.js"></scrip t>
      
      <script>
         var mainApp = angular.module("mainApp", []);
         
         mainApp.config(function($provide) {
            $provide.provider('MathService', function() {
               this.$get = function() {
                  var factory = {};
                  
                  factory.multiply = function(a, b) {
                     return a * b;
                  }
                  return factory;
               };
            });
         });
			
         mainApp.value("defaultInput", 5);
         
         mainApp.factory('MathService', function() {
            var factory = {};
            
            factory.multiply = function(a, b) {
               return a * b;
            }
            return factory;
         });
         
         mainApp.service('CalcService', function(MathService){
            this.square = function(a) {
               return MathService.multiply(a,a);
            }
         });
         
         mainApp.controller('CalcController', function($scope, CalcService, defaultInput) {
            $scope.number = defaultInput;
            $scope.result = CalcService.square($scope.number);

            $scope.square = function() {
               $scope.result = CalcService.square($scope.number);
            }
         });
			
      </scrip t>
      
   </body>
</html>
```

## AngularJS 服务(Service)
AngularJS 中你可以创建自己的服务，或使用内建服务。
什么是服务？
在 AngularJS 中，服务是一个函数或对象，可在你的 AngularJS 应用中使用。
AngularJS 内建了30 多个服务。
有个 $location 服务，它可以返回当前页面的 URL 地址。
实例
```python
var app = angular.module('myApp', []);
app.controller('customersCtrl', function($scope, $location) {
    $scope.myUrl = $location.absUrl();
});
```
$http 服务
$http 是 AngularJS 应用中最常用的服务。 服务向服务器发送请求，应用响应服务器传送过来的数据。
```python
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http.get("welcome.htm").then(function (response) {
        $scope.myWelcome = response.data;
    });
});
```

$timeout 服务
AngularJS $timeout 服务对应了 JS window.setTimeout 函数。
```python
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $timeout) {
    $scope.myHeader = "Hello World!";
    $timeout(function () {
        $scope.myHeader = "How are you today?";
    }, 2000);
});
```
$interval 服务
AngularJS $interval 服务对应了 JS window.setInterval 函数。

创建自定义服务
你可以创建访问自定义服务，链接到你的模块中：
创建名为hexafy 的访问:
```python
app.service('hexafy', function() {
    this.myFunc = function (x) {
        return x.toString(16);
    }
});
```
要使用访问自定义服务，需要在定义过滤器的时候独立添加:
实例
使用自定义的的服务 hexafy 将一个数字转换为16进制数:

```python
app.controller('myCtrl', function($scope, hexafy) {
    $scope.hex = hexafy.myFunc(255);
});
```
过滤器中，使用自定义服务
当你创建了自定义服务，并连接到你的应用上后，你可以在控制器，指令，过滤器或其他服务中使用它。
在过滤器 myFormat 中使用服务 hexafy:
```python
app.filter('myFormat',['hexafy', function(hexafy) {
    return function(x) {
        return hexafy.myFunc(x);
    };
}]);
```
在对象数组中获取值时你可以使用过滤器：
创建服务 hexafy:
```python
<ul>
<li ng-repeat="x in counts">{{x | myFormat}}</li>
</ul>
```
## 应用
ng-app 指令位于应用的根元素下。
对于单页Web应用（single page web application，SPA），应用的根通常为 <html> 元素。
一个或多个 ng-controller 指令定义了应用的控制器。每个控制器有他自己的作用域：: 定义的 HTML 元素。
AngularJS 在 HTML DOMContentLoaded 事件中自动开始。如果找到 ng-app 指令 ， AngularJS 载入指令中的模块，并将 ng-app 作为应用的根进行编译。
应用的根可以是整个页面，或者页面的一小部分，如果是一小部分会更快编译和执行。


## AngularJS中的依赖注入
1、angular.module()创建、获取、注册angular中的模块
```python
/ 传递参数不止一个,代表新建模块;空数组代表该模块不依赖其他模块
var createModule = angular.module("myModule", []);

// 只有一个参数(模块名),代表获取模块
// 如果模块不存在,angular框架会抛异常
var getModule = angular.module("myModule");

// true,都是同一个模块
alert(createModule == getModule);
```

该函数既可以创建新的模块，也可以获取已有模块，是创建还是获取，通过参数的个数来区分。
angular.module(name, [requires], [configFn]);

name：字符串类型，代表模块的名称；

requires：字符串的数组，代表该模块依赖的其他模块列表，如果不依赖其他模块，用空数组即可；

configFn：用来对该模块进行一些配置。

4、angular中三种声明依赖的方式

angular提供了3种获取依赖的方式：inference、annotation、inline方式。

```python
// 创建myModule模块、注册服务
var myModule = angular.module('myModule', []);
myModule.service('myService', function() {
			this.my = 0;
});

// 获取injector
var injector = angular.injector(["myModule"]);

// 第一种inference
injector.invoke(function(myService){alert(myService.my);});

// 第二种annotation
function explicit(serviceA) {alert(serviceA.my);};
explicit.$inject = ['myService'];
injector.invoke(explicit);

// 第三种inline
injector.invoke(['myService', function(serviceA){alert(serviceA.my);}]);
```
```python
http://blog.csdn.net/renfufei/article/details/19038123
```
依赖注解 | Dependency Annotation
injector 怎么知道需要注入何种 service 呢?
为了解决依赖关系,应用程序开发者需要提供 injector 需要的 annotation 信息。在 Angular 中,某些API函数通过使用 injector 来调用,请按照API文档。injector 需要知道注入哪些服务给函数。下面是通过 service name 信息对代码进行注解的三种等价方式。他们都是等价的，你可以在适当的地方互换使用.

推断依赖关系 | Inferring Dependencies
最简单的获取依赖的方式,就是让函数参数名和依赖的名字一致。
```python
function MyController($scope, greeter) {  
  ...  
}  
```
给定一个 function, injector 通过检查函数声明和提取参数名称可以推断出 service 的名称 。在上面的例子中, $scope 和 greeter 是需要注入 function 的两个 services。
虽然简单直接, 但这种方法在 JavaScript 压缩/混淆 时会失效,因为会重命名方法的参数名。这使得这种注解方式只适用于 pretotyping, 或者 demo 程序中。

$inject 注解 | $inject Annotation
为了可以在压缩代码后依然可以注入正确的 services, 函数需要通过 $inject 属性来注解. $inject 属性是一个数组,包含 需要注入的 service 名字.

var MyController = function(renamed$scope, renamedGreeter) {  
}  
MyController['$inject'] = ['$scope', 'greeter'];  


在这种情况下,$inject数组中的值的顺序必须和要注入的参数的顺序一致。使用上面的代码片段作为一个例子, '$scope' 将注入到 “renamed$scope”, 而“greeter” 将注入到 “renamedGreeter”。再次提醒注意 $inject 注解必须和 函数声明时的实际参数保持同步(顺序,个数...)。
对于 controller 声明,这种注解方法是很有用的,因为它将注解信息赋给了 function。

简介AngularJS中使用factory和service的方法
AngularJS支持使用服务的体系结构“关注点分离”的概念。服务是JavaScript函数，并负责只做一个特定的任务。这也使得他们即维护和测试的单独实体。控制器，过滤器可以调用它们作为需求的基础。服务使用AngularJS的依赖注入机制注入正常。
AngularJS提供例如许多内在的服务，如：$http, $route, $window, $location等。每个服务负责例如一个特定的任务，$http是用来创建AJAX调用，以获得服务器的数据。 $route用来定义路由信息等。内置的服务总是前缀$符号。
有两种方法来创建服务。
    工厂
    服务
使用工厂方法
使用工厂方法，我们先定义一个工厂，然后分配方法给它。
```python
   var mainApp = angular.module("mainApp", []);
   mainApp.factory('MathService', function() {   
     var factory = {}; 
     factory.multiply = function(a, b) {
      return a * b 
     }
     return factory;
   }); 
   ```
   使用服务方法
使用服务的方法，我们定义了一个服务，然后分配方法。还注入已经可用的服务。
```python
mainApp.service('CalcService', function(MathService){
  this.square = function(a) { 
 return MathService.multiply(a,a); 
 }
});
```

$rootScope

一个网站中有很多页面都要判断登录状态 ， 我之前是直接在启动的时候查登录状态，并挂在$rootScope上，然后各个页面判断登录状态的操作逻辑直接与$rootScope对应属性双向绑定。 这样就不用事件传递了 ，如果涉及逻辑操作 ，还是建议放在服务里面。 services 相当于业务处理模块，而$rootScope 相当于全局变量。
scope是AngularJS中的作用域(其实就是存储数据的地方)，很类似JavaScript的原型链 。搜索的时候，优先找自己的scope，如果没有找到就沿着作用域链向上搜索，直至到达根作用域rootScope。
　　$rootScope是由angularJS加载模块的时候自动创建的，每个模块只会有1个rootScope。rootScope创建好会以服务的形式加入到 $injector中。也就是说通过 $injector.get("$ rootScope ");能够获取到某个模块的根作用域。更准确的来说，$rootScope是由angularJS的核心模块ng创建的。

　　scope是html和单个controller之间的桥梁，数据绑定就靠他了。rootscope是各个controller中scope的桥梁。用rootscope定义的值，可以在各个controller中使用

事件：

$stateChangeError

路由状态变化发生错误时触发的事件。参数有：event，toState，toParams，fromState，fromParams，error。以上根据字面意思即可理解，哈哈。

$stateChangeStart

路由状态变化发生前触发的事件。参数有：event，toState，toParams，fromState，fromParams。

$stateChangeSuccess

路由状态变化正确时触发的事件。参数有：event，toState，toParams，fromState，fromParams。

$stateNotFound

路由状态没找到的时候触发的事件。参数有：event，unfoundState，fromState，fromParams。

state(name,stateConfig);

注册一个状态，并给定其配置。
参数：

name：状态的名称。

stateConfig：状态配置对象。配置具有以下各项属性：

template： string/function，html模板字符串，或者一个返回html模板字符串的函数。

templateUrl：string/function，模板路径的字符串，或者返回模板路径字符串的函数。

templateProvider：function，返回html模板字符串或模板路径的服务。

controller：string/function，新注册一个控制器函数或者一个已注册的控制器的名称字符串。

controllerProvider：function，返回控制器或者控制器名称的服务

controllerAs：string，控制器别名。

parent：string/object，手动指定该状态的父级。

resolve：object，将会被注入controller去执行的函数，<string,function>形式。

url：string，当前状态的对应url。

views：object，视图展示的配置。<string,object>形式。

abstract：boolean，一个永远不会被激活的抽象的状态，但可以给其子级提供特性的继承。默认是true。

onEnter：function，当进入一个状态后的回调函数。

onExit：function，当退出一个状态后的回调函数。

reloadOnSearch：boolean，如果为false，那么当一个search/query参数改变时不会触发相同的状态，用于当你修改$location.search()的时候不想重新加载页面。默认为true。

data：object，任意对象数据，用于自定义配置。继承父级状态的data属性。换句话说，通过原型继承可以达到添加一个data数据从而整个树结构都能获取到。

params：url里的参数值，通过它可以实现页面间的参数传递。

AngularJS 中的 controllerAs

Controller 在 AngularJS 应用中可以说是无处不在， 可以在 html 中通过 ngController 指令来指定 Controller ， 语法为：

<ANY
    ng-controller="expression">
    ...
</ANY>
在 ngRoute 模块中使用， 语法为：

$routeProvider
    .when('/my-url', {
        controller: 'MyController'
    });
在 ui.route 模块中使用， 语法为：

$stateProvider
    .state('myState', {
        controller: 'MyController'
    })
上面用法在 AngularJS 的社区、示例程序中非常普遍。 但是， 有一个细节可能很多人没有注意到， 那就是 controllerAs ， 上面的三种用法还可以分别这样使用：

<ANY
    ng-controller="expression as myExpr">
    ...
</ANY>
$routeProvider
    .when('/my-url', {
        controller: 'MyController',
        controllerAs: 'ctrl'
    });
$stateProvider
    .state('myState', {
        controller: 'MyController',
        controllerAs: 'ctrl'
    })
那么， 使用了 controllerAs 有什么区别呢？ 在 AngularJS 的文档中是这样说的：

one binds methods and properties directly onto the controller using this: ng-controller=”SettingsController1 as settings”
one injects $scope into the controller: ng-controller=”SettingsController2”
上面的意思是说， 就是使用 controllerAs 将直接绑定 Controller 的属性和方法， 而不使用 controllerAs 将绑定到为 Controller 注入的 $scope 参数， 下面用一个具体的例子来说明一下：

不使用 controllerAs 指令时， 通常我们这样做：
```python

angular
    .module('app', []).
    controller('TestController', TestController);

TestController.$inject = ['$scope', '$window'];

function TestController($scope, $window) {
    $scope.name = 'beginor';
    
    $scope.greet = greet;
    
    function greet() {
        $window.alert('Hello, ' + $scope.name);
    }
}
<div ng-Controller="TestController">
    <label>Name:
        <input type="text" ng-model="name" />
    </label>
    <button type="button" ng-click="greet()">
</div>
```
在 HTML 视图中， 我们绑定的是 $scope 对象的属性和方法， 而不是 TestController 的实例。

上面的例子在使用 controllerAs 时， 可以修改成这样：
```python
angular
    .module('app', []).
    controller('TestController', TestController);

TestController.$inject = ['$window'];

function TestController($window) {
    this.name = 'beginor';
    this.$window = $window;
}

TestController.prototype.greet = function () {
    this.$window.alert('Hello, ' + this.name);
}
<div ng-Controller="TestController as vm">
    <label>Name:
        <input type="text" ng-model="vm.name" />
    </label>
    <button type="button" ng-click="vm.greet()">
</div>
```
看到区别了吧， 使用 controllerAs 时， 可以将 Controller 定义成 Javascript 的原型类， 在 HTML 视图中直接绑定原型类的属性和方法。

这样做的优点是：

可以使用 Javascript 的原型类， 我们可以使用更加高级的 ES6 或者 TypeScript 来编写 Controller ；
避开了所谓的 child scope 原型继承带来的一些问题， 具体可以 参考这里 ；


angular的uiRouter服务学习(5) --- $state.includes()方法
$state.includes方法用于判断当前激活状态是否是指定的状态或者是指定状态的子状态.

$state.includes(stateOrName,params,options)

$state.includes方法接受三个参数,其中第二和第三个都不知道是干啥的...估计也不太用得到,就暂时不管了...

stateOrName:字符串(必填). 是一个状态的名字.

比如当前的激活状态是 "contacts.details.item" 

如下调用:

```python
$state.includes("contacts");                              //返回true
$state.includes("contacts.details");                      //返回true
$state.includes("contacts.details.item");                 //返回true
$state.includes("detail");                                //返回undefined
$state.includes("item");                                  //返回undefined
```
也可以使用glob语法:

复制代码
```python
$state.$current.name = 'contacts.details.item.url';
 
$state.includes("*.details.*.*"); // returns true
$state.includes("*.details.**"); // returns true
$state.includes("**.item.**"); // returns true
$state.includes("*.details.item.url"); // returns true
$state.includes("*.details.*.url"); // returns true
$state.includes("*.details.*"); // returns undefined
$state.includes("item.**"); // returns undefined
```
复制代码
可以用于激活某个tab,让当前项高亮显示:
```python
<li ng-class="{active:state.includes('dashboard.report')}"><a ui-sref="dashboard.report">Reports</a></li>
```
需要注意的是,在表达式里直接用$state是不行的,需要在控制器中把$state赋值给$scope下的变量.这样在表达式里才能使用:
```python
    $stateProvider.state('dashboard',{
        url:'/dashboard',
        templateUrl:'./tpls/dashboard.html',
        controller:function($scope,$state){
            $scope.state = $state;              
        }
    })
```

AngularJS ng - swi tch 指令
根据选中的值显示对应部分:
```python
<div ng-switch="myVar">
  <div ng-switch-when="runoob">
     <h1>菜鸟教程</h1>
     <p>欢迎访问菜鸟教程</p>
  </div>
  <div ng-switch-when="google">
     <h1>Google</h1>
     <p>欢迎访问Google</p>
  </div>
  <div ng-switch-when="taobao">
     <h1>淘宝</h1>
     <p>欢迎访问淘宝</p>
  </div>
  <div ng-switch-default>
     <h1>切换</h1>
     <p>选择不同选项显示对应的值。</p>
  </div>
</div>
```
定义和用法
ng-sw itch 指令根据表达式显示或隐藏对应的部分。
对应的子元素使用 ng-sw itch-when 指令，如果匹配选中选择显示，其他为匹配的则移除。
你可以通过使用 ng-s witch-default 指令设置默认选项，如果都没有匹配的情况，默认选项会显示。
语法
```python
<element ng-switch="expression">
  <element ng-switch-when="value"></element>
  <element ng-switch-when="value"></element>
  <element ng-switch-when="value"></element>
  <element ng-switch-default></element>
</element>
<form> 元素支持该属性。
```

在 Angularjs 中 ui-sref 和 $state.go 如何传递参数
ui-sref、$state.go 的区别

ui-sref 一般使用在 < a >...< / a >；

<a ui-sref="message-list">消息中心< / a >

$state.go('someState')一般使用在 controller里面；

.controller('firstCtrl', function($scope, $state) {
      $state.go('login');
 });
这两个本质上是一样的东西，我们看ui-sref的源码：

复制代码
...
element.bind("click", function(e) {
    var button = e.which || e.button;
    if ( !(button > 1 || e.ctrlKey || e.metaKey || e.shiftKey || element.attr('target')) ) {

      var transition = $timeout(function() {
        // HERE we call $state.go inside of ui-sref
        $state.go(ref.state, params, options);
      });
复制代码
ui-sref最后调用的还是$state.go()方法
首先，要在目标页面定义接受的参数：
http://images2015.cnblogs.com/blog/337212/201603/337212-20160318181804412-426985465.png
传参，

ui-sref:
http://images2015.cnblogs.com/blog/337212/201603/337212-20160318182142693-1979913166.png
http://images2015.cnblogs.com/blog/337212/201603/337212-20160318182359646-1518874208.png



