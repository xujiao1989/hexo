---
title: gulp小白文
date: 2017-03-21
tags:
  - 前端开发
  - 个人成长
  - 其他
  - 自动化构建
categories: 其他
---

## 1. 关于gulp

### 1.1 gulp是什么

gulp是一款提高我们开发效率的工具。比如：

+ 自动帮我们添加浏览器兼容前缀
+ 自动刷新浏览器
+ 我们想要使用ES6，gulp还可以帮我们把ES6代码编译转换成浏览器可以用的js

而这一切并不是gulp本身完成的，而是一系列的gulp插件来帮忙完成的。


### 1.2 gulp环境简单配置

+ 首先你得有node环境（去下载node并安装好，内置了npm包管理工具）
+ 切到工程目录下，npm init 根据提示初始化package.json文件，如下图所示操作，会在项目目录下生成一个package.json文件(package.json文件也可以手动添加)
	
    ![package.json](/images/gulp1.png)
    
	生成的package.json文件如下图所示
	
	![package.json](/images/gulp2.png)

+ 安装gulp 

		npm install gulp -g //全局安装
		npm install gulp --save-dev //工程目录下会生成一个node_modules文件，里面保存着开发所需要的依赖

	当你为你的模块安装一个依赖模块时，正常情况下你得先安装他们（在模块根目录下npm install module-name），然后连同版本号手动将他们添加到模块配置文件package.json中的依赖里（devDependencies）--摘自网络

	+ -save和save-dev可以省掉你手动修改package.json文件的步骤。
	+ -save 自动把模块和版本号添加到dependencies部分
	+ -save-dve 自动把模块和版本号添加到devdependencies部分
	
	至于配置文件区分这俩部分， 是用于区别开发依赖模块和产品依赖模块， 以我见过的情况来看 devDepandencies主要是配置测试框架

+ 开始使用

	在项目目录下创建一个gulpfile.js文件，这个文件就gulp任务的入口文件,在gulpfile.js文件中添加以下代码段，在项目目录下执行gulp,可以看出命令行输出了“gulp任务执行啦~~~”

	```javascript
		
		var gulp = require('gulp');
		gulp.task("default",function(){
		    console.log("gulp任务执行啦~~~");
		})
	```

好吧，关于gulp的接口只有那么几个，自己去看一看

### 1.3 我自己的学习经验就是：

+ 正确认识gulp: 它并不是一个什么多么难的新知识，只要我们根据要求配合插件可以实现非常强大的功能。

+ 重要的是我们要知道有哪些插件，最好的就是我们在使用过程中想要用什么功能（压缩js，合并js，拷贝文件，简直不要太多）就去网上搜一下，各种插件就出来了

+ 自己可以根据任务搭建一套手脚架，提高开发效率


	
## 1.4 比较好的gulp插件

+ autoprefixer: 自动添加css兼容前缀
+ run-sequence：让任务船型执行
+ [这里很多资料，不骗你](https://github.com/Platform-CUF/use-gulp)

## 2. 项目中使用ES6的模块

```javascript
buildjs :(fromPath,toPath,name)=>{
    return browserify({
        entries: [fromPath],
        debug: true, // 告知Browserify在运行同时生成内联sourcemap用于调试
    }).transform("babelify", {presets: ["es2015"]})//转为ES5
        .bundle()//打包
        .pipe(source(name ? name : "app.js" ))
        .pipe(buffer()) // 缓存文件内容
        .pipe(gulp.dest(toPath));
}
```



## 3. 参考文章

+ [使用 gulp 搭建前端环境之CommonJs & ES6 模块化(中级篇)](http://div.io/topic/1506)


---
做一个勤于思考的人