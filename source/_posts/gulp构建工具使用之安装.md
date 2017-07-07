---
title: gulp构建工具使用之安装
date: 2016-09-07 15:22:36
categories: study_blog
tags: [构建工具,gulp,前端]
---

* ## 安装node.js

打开[node.js官网](https://nodejs.org/en/),根据自己的系统下载安装对应的node版本。  
安装完之后，分别输入
``` bash
$ node -v
$ npm -v
```
可以用来查看node与npm的版本
<!-- more -->
* ## 全局安装gulp

使用以下命令安装：
```bash
$ npm install -g gulp
```
不过在国内使用npm安装会非常的慢，有时候甚至安装失败，所以可以用npm的国内镜像[淘宝 NPM 镜像](https://npm.taobao.org/)，这是一个
完整的npmjs.org的镜像。使用方法在官网可以查看。  
使用说明：
```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
安装结束后，就可以用cnpm替代npm执行所有的操作。

* ## 关于package.json

package.json文件描述了一个NPM包的所有相关信息，包括作者、简介、包依赖、构建等信息。格式必须是严格的JSON格式。  
通常我们在创建一个NPM程序时，可以使用命令，通过交互式的命令，自动生成一个package.json文件，里面包含了常用的一些字段信息，但远不止这么简单。
通过完善package.json文件，我们可以让npm命令更好地为我们服务。  
创建package.json文件命令：
```bash
$ npm init
```
![package.json](http://od4k4xerr.bkt.clouddn.com/gulp_use_start_packageJsonShow.png)

* ## 项目安装gulp

### 安装

```bash
$ cnpm install --save-dev gulp
```
### 创建任务

然后新建一个gulp的主文件“gulpfile.js”，，我们可以在该文件中定义要执行的任务。  
在gulpfile.js中建立一个defualt任务
```javascript
var gulp = require('gulp');
gulp.task('default',function(){
    console.log('use gulp');
});
```
### 执行任务

直接在命令行中输入`gulp`命令即可运行defualt任务，如要执行指定任务则要加上任务名，如`gulp taskNmme`
![gulp task](http://od4k4xerr.bkt.clouddn.com/gulp_use_start_gulpTaskShow.png)