---
title: Jquery简写
date: 2016-12-15 14:49:37
tags:
---
## ~ function($) {}($) 
```python
var fn = function(opt){};
fn($);
```
## $this和$(this)
// this其实是一个Html 元素。
// $this 只是个变量名，加$是为说明其是个jquery对象。
// 而$(this)是个转换，将this表示的dom对象转为jquery对象，这样就可以使用jquery提供的方法操作。

## return this.each(function () {})
因为each返回的也是this对象，所以直接return this.each可以执行你的相关操作，还可以保持链式调用功能
因为this.each保证了遍历完成才执行下一个操作，否则迭代是延迟执行的，前面的插件并没有实际执行。
```python
jQuery.fn.test2= function(){ 
   this.css("background","#ff0");//这里面的this为jquery对象，而不是dom对象 
   return this.each(function(){ //遍历匹配的元素，此处的this表示为jquery对象，而不是dom对象 
    alert("this"+this+this.innerHTML); 
    //提示当前对象的dom节点名称,这里的this关键字都指向一个不同的DOM元素（每次都是一个不同的匹配元素）。 
     }); 
};
```
this.css(),this.each（）里面的this为jquery对象，但是alert里面this为dom对象.
为什么要return this.each()
先return this.each(),后调用each（）方法，而each（）方法返回jQuery对象，所以这样就可以继续链式操作了。
首先在JQ中,each是遍历一个数组,比如你$('.some')返回的不一定只是一个jq对象,有可能是个数组,好多个elements.
所以return this.each(){}是把所有你索引的对象都作用到这个插件下.
你若保证你的插件每次都只会用一个JQ对象,那么你可以直接return this.


## JavaScript prototype 属性
定义和用法
prototype 属性允许您向对象添加属性和方法
注意： Prototype 是全局属性，适用于所有的Javascript对象。
语法
object.prototype.name=value

## _.extend
_，在这里应该是接管了JQUERY，就是jQuery。(有不少人这样用，因为和$冲突)
_.extend方法是把指定的对象进行扩展(在这里就是document.body.style)
Query中需要用到$符号，如果其他js库（例如大名鼎鼎的prototype)也定义了$符号，那么就会造成冲突，会影响到js代码的正常执行。

## jquery call方法和apply方法
call方法: 
语法：call([thisObj[,arg1[, arg2[,   [,.argN]]]]]) 
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 
说明： 
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 

apply方法： 
语法：apply([thisObj[,argArray]]) 
定义：应用某一对象的一个方法，用另一个对象替换当前对象。 
说明： 
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。 


```python
  
    <script language="javascript"><!--
   
    /**定义一个animal类*/  
    function Animal(){   
    this.name = "Animal";   
    this.showName = function(){   
    alert(this.name);   
    }   
    }   
    /**定义一个Cat类*/  
    function Cat(){   
    this.name = "Cat";   
    }   
  
    /**创建两个类对象*/  
    var animal = new Animal();   
    var cat = new Cat();   
  
    //通过call或apply方法，将原本属于Animal对象的showName()方法交给当前对象cat来使用了。   
    //输入结果为"Cat"   
    animal.showName.call(cat,",");   
    //animal.showName.apply(cat,[]);   
      
    
// --></script> 
```