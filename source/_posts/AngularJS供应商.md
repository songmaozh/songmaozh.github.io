---
title: AngularJS供应商
date: 2017-01-19 10:21:50
tags:
---
引言
看了很多文章可能还是不太说得出AngularJS中的几个创建供应商(provider)的方法(factory(),service(),provider())到底有啥区别，啥时候该用啥，之前一直傻傻分不清楚，现在来总结一下。

下文中泛指统一用中文，英文即为特指$provide方法中对应方法创建出的东东

供应商==>泛指provider
服务==>泛指service

provider==>provider()方法创建的东东
service==>service()方法创建的东东

先说说供应商（$provide）
$provide服务负责告诉Angular如何创造一个新的可注入的东西：即服务。服务会被叫做供应商的东西来定义，你可以使用$provide来创建一个供应商。你需要使用$provide中的provider()方法来定义一个供应商，同时你也可以通过要求$provide被注入到一个应用的config函数中来获得$provide服务。

上面的描述是官方wiki的翻译版，如果有些绕的话，看下这张图：
https://segmentfault.com/img/bVm9Et

$provide是一个服务，在Auto模块中
这个服务下面有好多方法，是用来定义供应商
而供应商是用来提供服务的，被注入来注入去的东西就是供应商们提供的服务了
下面是一个例子：

myMod.config(function($provide) {
  $provide.provider('greeting', function() {
    this.$get = function() {
      return function(name) {
        alert("Hello, " + name);
      };
    };
  });
});
这个例子可以说是最终形态，先大概看下

定义供应商的方法们
AngularJS用$provide去定义一个供应商,这个$provide有5个用来创建供应商的方法：

constant
value
service
factory
provider
decorator 我没有说我也是，我只是路过o(╯□╰)o
Constant
定义常量用的，这货定义的值当然就不能被改变，它可以被注入到任何地方，但是不能被装饰器(decorator)装饰

var app = angular.module('app', []);
 
app.config(function ($provide) {
  $provide.constant('movieTitle', 'The Matrix');
});
 
app.controller('ctrl', function (movieTitle) {
  expect(movieTitle).toEqual('The Matrix');
});
语法糖：

app.constant('movieTitle', 'The Matrix');
Value
这货可以是string,number甚至function,它和constant的不同之处在于，它可以被修改，不能被注入到config中，但是它可以被decorator装饰

var app = angular.module('app', []);
 
app.config(function ($provide) {
  $provide.value('movieTitle', 'The Matrix')
});
 
app.controller('ctrl', function (movieTitle) {
  expect(movieTitle).toEqual('The Matrix');
})
语法糖：

app.value('movieTitle', 'The Matrix');
Service
它是一个可注入的构造器，在AngularJS中它是单例的，用它在Controller中通信或者共享数据都很合适

var app = angular.module('app' ,[]);
 
app.config(function ($provide) {
  $provide.service('movie', function () {
    this.title = 'The Matrix';
  });
});
 
app.controller('ctrl', function (movie) {
  expect(movie.title).toEqual('The Matrix');
});
语法糖：

app.service('movie', function () {
  this.title = 'The Matrix';
});
在service里面可以不用返回东西，因为AngularJS会调用new关键字来创建对象。但是返回一个自定义对象好像也不会出错。

Factory
它是一个可注入的function，它和service的区别就是：factory是普通function，而service是一个构造器(constructor)，这样Angular在调用service时会用new关键字，而调用factory时只是调用普通的function，所以factory可以返回任何东西，而service可以不返回(可查阅new关键字的作用)

var app = angular.module('app', []);
 
app.config(function ($provide) {
  $provide.factory('movie', function () {
    return {
      title: 'The Matrix';
    }
  });
});
 
app.controller('ctrl', function (movie) {
  expect(movie.title).toEqual('The Matrix');
});
语法糖：

app.factory('movie', function () {
  return {
    title: 'The Matrix';
  }
});
factory可以返回任何东西，它实际上是一个只有$get方法的provider

Provider
provider是他们的老大，上面的几乎(除了constant)都是provider的封装，provider必须有一个$get方法，当然也可以说provider是一个可配置的factory

var app = angular.module('app', []);
 
app.provider('movie', function () {
  var version;
  return {
    setVersion: function (value) {
      version = value;
    },
    $get: function () {
      return {
          title: 'The Matrix' + ' ' + version
      }
    }
  }
});
 
app.config(function (movieProvider) {
  movieProvider.setVersion('Reloaded');
});
 
app.controller('ctrl', function (movie) {
  expect(movie.title).toEqual('The Matrix Reloaded');
});
注意这里config方法注入的是movieProvider，上面定义了一个供应商叫movie，但是注入到config中不能直接写movie，因为前文讲了注入的那个东西就是服务，是供应商提供出来的，而config中又只能注入供应商（两个例外是$provide和$injector），所以用驼峰命名法写成movieProvider，Angular就会帮你注入它的供应商。（更详细可参考文末官方wiki翻译版中的配置provider）

Decorator
这个比较特殊，它不是provider,它是用来装饰其他provider的，而前面也说过，他不能装饰Constant，因为实际上Constant不是通过provider()方法创建的。

var app = angular.module('app', []);
 
app.value('movieTitle', 'The Matrix');
 
app.config(function ($provide) {
  $provide.decorator('movieTitle', function ($delegate) {
    return $delegate + ' - starring Keanu Reeves';
  });
});
 
app.controller('myController', function (movieTitle) {
  expect(movieTitle).toEqual('The Matrix - starring Keanu Reeves');
});
总结
所有的供应商都只被实例化一次，也就说他们都是单例的
除了constant，所有的供应商都可以被装饰器(decorator)装饰
value就是一个简单的可注入的值
service是一个可注入的构造器
factory是一个可注入的方法
decorator可以修改或封装其他的供应商，当然除了constant
provider是一个可配置的factory
最后来看一张对比图：
https://segmentfault.com/img/bVnbg2

最近，在项目中遇到需要在 config 阶段中注入一些service的情况，然而 factory，service 还有 value 是不能在 config 中注入的，先看一个清单:
服务/阶段	provider	factory	service	value	constant
config阶段	Yes	No	No	No	Yes
run 阶段	Yes	Yes	Yes	Yes	Yes


注意，provider 在config阶段，注入的时候需要加上 provider 后缀，可以调用非 $get 返回的方法在 run 阶段注入的时候，无需加 provider 后缀，只能调用 $get 返回的方法