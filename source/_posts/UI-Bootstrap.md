---
title: UI-Bootstrap
date: 2017-01-16 13:45:19
tags:
---
在ui-Bootstrap中，Buttons控件和Dropdown控件与form表单中的按钮和下拉框名字很像，但实际上这两个控件有新的含义。

先说Buttons，它是一组按钮，用来实现form表单中的单选框和复选框的功能，样式上可以自定义，功能也可以扩展，可以替代单选框和复选框。
buttons控件使用uib-btn-checkbox和uib-btn-radio指令，这两个指令可以加在<input >上，也可以加在<button>上，甚至可以加在<lable>上。

对于复选框，可以设置btn-checkbox-false和btn-checkbox-true表示复选框未选中和选中时的值（默认是false和true）。

对于单选框，一组单选框需要绑定同一个ng-model，并且使用uib-btn-radio指定单选框选中时的值。

单选框还有两个可选的属性：

属性名

默认值

备注

uncheckable

 

增加这个属性表示单选框选中状态下再次点击时,单选框变为未选中(单选框变成复选框了)

uib-uncheckable

null

为true时效果等于增加uncheckable属性

再说Dropdown，从外观上看似乎是表单控件< select>

http://images2015.cnblogs.com/blog/234349/201607/234349-20160702224158234-1791642801.png

但是实际上这个控件的主要功能是做导航菜单，因此在模板中使用button和ul元素的组合来表现菜单项。
具体来说，dropdown包括三部分：

1. uib-dropdown 表示当前元素是一个菜单

2. uib-dropdown-toggle 一个展开/收起菜单的按钮。这是可选的部分。

3. uib-dropdown-menu 表示具体的菜单项

uib-dropdown的属性有：
