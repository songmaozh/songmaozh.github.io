---
title: jhipster搭建Mciroservice
date: 2017-01-12 13:41:34
tags:
---

简介
jhipster简单来说是一个基于nodejs+yeoman的java代码生成器。往大了说是基于java的一套微服务解决方案。请注意是一整套的微服务解决方案。jhipster在整个程序架构上都做好了整合，包括前端mvvm框架（angularjs），前端构建工具（gulp）到后端的微服务框架（spring cloud）和hibernate/mongodb，再到单元测试/ui测试。
毫不客气的说 ：学会了这套框架，你就是程序开发/程序架构界的潮男。对,hipster的意思就是：追求新奇的人。

demo
下面跟着我来一步一步的来见证奇迹。

1.安装nodejs。

2.安装yeoman/bower/gulp npm install -g yo bower gulp-cli

3.安装jhipster npm install -g generator-jhipster

是不是被gfw艹翻了？哈哈哈

## 生成单机服务app
cd到你想存放代码的路径，然后运行：yo jhipster
第一个选择很重要，项目类型要选择第一个，单机服务
后面的根据实际情况，选择就可以。失败了也没关系，删掉文件夹重新来过。

------生成成功后运行 
./mvnw 或者gradlew下载依赖包。
./mvnw (on Mac OS X/Linux) of mvnw (on Windows)


## 生成mciroservice app
生成基础架构

cd到你想存放代码的路径，然后运行：yo jhipster

这时候jhipster向导就会启动了,如图：
![image](http://images2015.cnblogs.com/blog/33454/201607/33454-20160722105819779-322820644.png)

第一个选择很重要，项目类型要选择microservice application
后面的根据实际情况，选择就可以。失败了也没关系，删掉文件夹重新来过。

------生成成功后运行 ./mvnw 或者gradlew下载依赖包。

jhipster是可以生成实体和实体的增删改查带分页的

运行yo jhipster:entity <entityName>来启动实体生成向导。

然后跟着向导输入信息。

## 生成microservie gateway
生成基础架构

继续运行：yo jhipster
第一个选择很重要，项目类型要选择*microservice gateway

这个时候如果还被gfw折磨，你应该考虑ss或者vpn了。

生成实体

运行yo jhipster:entity <entityName>来启动实体生成向导。

然后跟着向导输入信息。

此处需要注意：

1.询问是否选择存在的app时 选择是

2.<entityName>需要时在app中生成过的

## 运行 jhipster registry
jhipster registry是一个基于spring cloud的配置中心，jhipster的微服务架构依赖此程序。

1 从github下载源码https://github.com/jhipster/jhipster-registry

2 cd 到解压目录 然后运行 ./mvnw或者gradlew 启动应用

浏览器地址：http://127.0.0.1:8761/.
效果如图
![image](http://images2015.cnblogs.com/blog/33454/201607/33454-20160722105802779-429391766.png)
默认用户名：密码  admin:admin

这个时候就可以启动app和gateway了。

cd到刚才存放microservice app的目录 运行./

cd到刚才存放microservice gateway的目录 运行./mvnw

然后打开浏览器见证奇迹
