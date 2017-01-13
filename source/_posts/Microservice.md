---
title: Microservice
date: 2017-01-13 10:14:24
tags:
---
## Microservice 
什么是微服务
Martin Fowler是国际著名的OO专家，敏捷开发方法的创始人之一，现为ThoughtWorks公司的首席科学家.福勒（Martin Fowler），在面向对象分析设计、UML、模式、软件开发方法学、XP、重构等方面，都是世界顶级的专家，现为Thought Works公司的首席科学家。Thought Works是一家从事企业应用开发和集成的公司。早在20世纪80年代，Fowler就是使用对象技术构建多层企业应用的倡导者，他著有几本经典书籍： 《企业应用架构模式》、《UML精粹》和《重构》等。—— 百度百科

先来看看传统的web开发方式，通过对比比较容易理解什么是Microservice Architecture。和Microservice相对应的，这种方式一般被称为Monolithic（比较难传神的翻译）。所有的功能打包在一个 WAR包里，基本没有外部依赖（除了容器），部署在一个JEE容器（Tomcat，JBoss，WebLogic）里，包含了 DO/DAO，Service，UI等所有逻辑。
![image](http://img3.tbcdn.cn/L1/461/1/cb87aabb9b184df0edd6769ef877b4b16b200855.png)
Monolithic比较适合小项目，优点是：

开发简单直接，集中式管理

基本不会重复开发

功能都在本地，没有分布式的管理开销和调用开销

它的缺点也非常明显，特别对于互联网公司来说（不一一列举了）：

开发效率低：所有的开发在一个项目改代码，递交代码相互等待，代码冲突不断

代码维护难：代码功能耦合在一起，新人不知道何从下手

部署不灵活：构建时间长，任何小修改必须重新构建整个项目，这个过程往往很长

稳定性不高：一个微不足道的小问题，可以导致整个应用挂掉

扩展性不够：无法满足高并发情况下的业务需求

所以，现在主流的设计一般会采用Microservice Architecture，就是基于微服务的架构。简单来说， 微服务的目的是有效的拆分应用，实现敏捷开发和部署 。
 ![image](http://img3.tbcdn.cn/L1/461/1/6a2474878e4c1000335770fe64269269f9211d17.png)
 用《The art of scalability》一书里提到的scale cube比较容易理解如何拆分。你看，我们叫分库分表，别人总结成了scale cube，这就是抽象的能力啊，把复杂的东西用最简单的概念解释和总结。X轴代表运行多个负载均衡器之后运行的实例，Y轴代表将应用进一步分解为微服务 （分库），数据量大时，还可以用Z轴将服务按数据分区（分表）
 ![image](http://img4.tbcdn.cn/L1/461/1/238adf07b6afdc6ae246e2da83f83ce2e144cbeb.png)

 先看看最官方的定义吧

The microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. These services are **built around business capabilities** and independently deployable by fully automated deployment machinery. There is a bare minimum of centralized management of these services , which may be written in different programming languages and use different data storage technologies.
-- James Lewis and Martin Fowler

把Martin老头的定义大概的翻译一下就是下面几条，这个定义还是太抽象是不是，那就对了，就是要务虚，都说明白了谁还找他付费咨询啊，这么贵。
1. 一些列的独立的服务共同组成系统
2. 单独部署，跑在自己的进程里
3. 每个服务为独立的业务开发
4. 分布式的管理

Martin自己也说了，每个人对微服务都可以有自己的理解，不过大概的标准还是有一些的。

分布式服务组成的系统

按照业务而不是技术来划分组织

做有生命的产品而不是项目

Smart endpoints and dumb pipes（我的理解是强服务个体和弱通信）

自动化运维（DevOps）

容错

快速演化

SOA vs Microservice

除了Smart endpoints and dumb pipes都很容易理解对吗？相信很多人都会问一个问题，这是不是就是SOA换了个概念，挂羊头卖狗肉啊，有说法把Microservice叫成 Lightway SOA。也有很多传统砖家跳出来说Microservice就是SOA。其实Martin也没否认SOA和Microservice的关系。

我个人理解，Microservice是SOA的传承，但一个最本质的区别就在于Smart endpoints and dumb pipes，或者说是真正的分布式的、去中心化的。Smart endpoints and dumb pipes本质就是去ESB，把所有的“思考”逻辑包括路由、消息解析等放在服务内部（Smart endpoints），去掉一个大一统的ESB，服务间轻（dumb pipes）通信，是比SOA更彻底的拆分。

客户端如何访问这些服务？

原来的Monolithic方式开发，所有的服务都是本地的，UI可以直接调用，现在按功能拆分成独立的服务，跑在独立的一般都在独立的虚拟机上的 Java进程了。客户端UI如何访问他的？后台有N个服务，前台就需要记住管理N个服务，一个服务下线/更新/升级，前台就要重新部署，这明显不服务我们 拆分的理念，特别当前台是移动应用的时候，通常业务变化的节奏更快。另外，N个小服务的调用也是一个不小的网络开销。还有一般微服务在系统内部，通常是无 状态的，用户登录信息和权限管理最好有一个统一的地方维护管理（OAuth）。

所以，一般在后台N个服务和UI之间一般会一个代理或者叫API Gateway，他的作用包括

提供统一服务入口，让微服务对前台透明

聚合后台的服务，节省流量，提升性能

提供安全，过滤，流控等API管理功能

我的理解其实这个API Gateway可以有很多广义的实现办法，可以是一个软硬一体的盒子，也可以是一个简单的MVC框架，甚至是一个Node.js的服务端。他们最重要的作 用是为前台（通常是移动应用）提供后台服务的聚合，提供一个统一的服务出口，解除他们之间的耦合，不过API Gateway也有可能成为单点故障点或者性能的瓶颈。

一般用过Taobao Open Platform的就能很容易的体会，TAO就是这个API Gateway。
![image](http://img2.tbcdn.cn/L1/461/1/4da28f2382d64d39ee4942c51636af31e9cc1d0b.png)

服务之间如何通信？

因为所有的微服务都是独立的Java进程跑在独立的虚拟机上，所以服务间的通行就是IPC（inter process communication），已经有很多成熟的方案。现在基本最通用的有两种方式。这几种方式，展开来讲都可以写本书，而且大家一般都比较熟悉细节了， 就不展开讲了。

同步调用

REST（JAX-RS，Spring Boot）

RPC（Thrift, Dubbo）

异步消息调用(Kafka, Notify, MetaQ)
![image](http://img2.tbcdn.cn/L1/461/1/d7e9a881c8940c216e6c1d8cb3bbbe7407e1e63b.png)

https://www.oschina.net/news/70121/microservice