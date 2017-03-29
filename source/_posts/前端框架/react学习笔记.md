---
title: react学习笔记
date: 2017-03-13
tags:
  - 前端开发
  - 个人成长
  - JavaScript
  - 前端框架
  - MVVM
categories: 前端框架
---

## react简介

react不是一个传统的MVC/MVVM的框架，它主要集中在view层面上。









## react环境配置

### babel
babel是一个多用途的JavaScript编译器，把最新版本的JavaScript变成成当下可以执行的版本。

+ 安装babel cli : npm install bable-cli -g
+ babel是通过安装插件或者预设来编译代码的
    + 创建一个名为.babelrc，它的主要作用是：
    + 安装babel-preset-es2015插件将es6编译成es5
    + .babelrc中将preset数组中加上'es2015'

+ babel的核心就是利用一系列的plugin来管理编译规则


### 前端组件化方案

组件和模块的理解：模块更多是语言层面的，而组件更多是业务层面的，组件相对而言比较完整，组件对外依赖越少越好，在设计组件的时候需要考虑可拓展性。

> JavaScript的模块化方案

大体经历了三个阶段：全局变量+命名空间--AMD&COMMONJS--ES6


> 单JavaScript入口组件

基于webpack等打包工具的流程，我们将平等的对待一切资源，最后打包成一个可执行的js文件作为组件的入口。


react推荐通过webpack或者browserify进行应用的构建，搭配对应的loader或者plugin可以实现通过JavaScript入口文件统一管理依赖资源。



## 学习资料

+ 《react全栈》这本书


## 学习路线

+ 2017年3月22日
    + 《react全栈》第五章
    + [react设计思想](http://cn.redux.js.org/docs/react-redux/api.html)
 

---
做一个勤于思考的人