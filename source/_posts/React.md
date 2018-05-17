---
title: React
date: 2017-02-28 13:29:12
tags:
---
http://www.infoq.com/cn/articles/subversion-front-end-ui-development-framework-react

如果你像在90年代那样写过服务器端Render的纯Web页面那么应该知道，服务器端所要做的就是根据数据Render出HTML送到浏览器端。如果这时因为用户的一个点击需要改变某个状态文字，那么也是通过刷新整个页面来完成的。服务器端并不需要知道是哪一小段HTML发生了变化，而只需要根据数据刷新整个页面。换句话说，任何UI的变化都是通过整体刷新来完成的。而React将这种开发模式以高性能的方式带到了前端，每做一点界面的更新，你都可以认为刷新了整个页面。至于如何进行局部更新以保证性能，则是React框架要完成的事情。

虚拟DOM不仅带来了简单的UI开发逻辑，同时也带来了组件化开发的思想，所谓组件，即封装起来的具有独立功能的UI部件。React推荐以组件的方式去重新思考UI构成，将UI上每一个功能相对独立的模块定义成组件，然后将小的组件通过组合或者嵌套的方式构成大的组件，最终完成整体UI的构建。例如，Facebook的instagram.com整站都采用了React来开发，整个页面就是一个大的组件，其中包含了嵌套的大量其它组件，大家有兴趣可以看下它背后的代码。

如果说MVC的思想让你做到视图-数据-控制器的分离，那么组件化的思考方式则是带来了UI功能模块之间的分离。我们通过一个典型的Blog评论界面来看MVC和组件化开发思路的区别。

对于MVC开发模式来说，开发者将三者定义成不同的类，实现了表现，数据，控制的分离。开发者更多的是从技术的角度来对UI进行拆分，实现松耦合。


对于React而言，则完全是一个新的思路，开发者从功能的角度出发，将UI分成不同的组件，每个组件都独立封装。

通过这种方式，每个组件的UI和逻辑都定义在组件内部，和外部完全通过API来交互，通过组合的方式来实现复杂的功能。React认为一个组件应该具有如下特征：

（1）可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，一个复杂的UI可以拆分成多个简单的UI组件；

（2）可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个UI场景；

（3）可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护；

（4）可测试（Testable）：因为每个组件都是独立的，那么对于各个组件分别测试显然要比对于整个UI进行测试容易的多。


多语 React-intl

http://www.tuicool.com/articles/IbINN3R

React+ES6+Webpack深入浅出
http://www.cnblogs.com/chenziyu-blog/p/5675086.html

index.jsx为

import React from 'react';
import { render } from 'react-dom';
import { Router, browserHistory } from 'react-router'
import routes from './components/router/router.jsx';

render(
  <Router routes={routes} history={browserHistory}/>,
  document.getElementById('example')
)
webpack.config.js配置为

      { test: /\.js$/, exclude:/node_modules/, loader: 'babel-loader'},
      { test: /\.jsx$/, exclude: /node_modules/, loader: 'babel-loader!jsx-loader?harmony' }
错误提示

ERROR in ./index.jsx
Module build failed: Error: Parse Error: Line 1: Illegal import declaration
2016年06月21日提问 评论 邀请回答  编辑  更多
查看全部 4 个回答

答案对人有帮助，有参考价值 0 答案没帮助，是错误的答案，答非所问
对于 jsx，你的配置的策略现在是先经过 jsx-loader ，然后再经过 babel-loader，然而 jsx-loader 并不能解析 es6 的 import 语法，因此报错。解决的策略上面已经提到，就是直接使用 babel-loader，不再使用 jsx-loader，目前 babel 已经非常完备地支持了 es2015/react 和一些 es7 的语法，完全不再需要 jsx-loader 了。
2016年06月21日回答  评论 编辑

紅白
1.3k 声望
推荐答案

答案对人有帮助，有参考价值 0 答案没帮助，是错误的答案，答非所问

已采纳
jsx-loader 已经废弃了，统一使用 babel-loader@>=6, 需要配置 babelrc 参见：https://babeljs.io/docs/usage/babelrc/

.babelrc 文件

{
  presets: [
    "es2015",
    "react"
  ]
}
或直接配置在 package.json 里

{
  "babel": {
    presets: ["es2015", "react"]
  }
}