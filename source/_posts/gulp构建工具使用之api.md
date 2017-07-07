---
title: gulp构建工具使用之api
date: 2016-09-09 19:46:09
categories: study_blog
tags: [构建工具,gulp,前端]
---
gulp重要的api有四个，分别是`gulp.src()`、`gulp.dest()`、`gulp.task()`、`gulp.watch()`，其他的参见
[官方文档](https://github.com/gulpjs/gulp/blob/master/docs/API.md)

<!-- more -->

* ## gulp.src()

gulp.src()方法是用来获取流的，这个流里的内容并不是原始的文件流，而是一个虚拟文件对象流(Vinyl files)，
这个虚拟文件对象中存储着原始文件的路径、文件名、内容等信息。  
__什么是流：__  
在Gulp中，使用的是Nodejs中的stream(流)，先获取到需要的stream，然后可以通过stream的pipe()方法把流导入到你想要的地方，比如Gulp的插件中，经过插件处理后的流又可以继续导入到其他插件中，当然也可以把流写入到文件中。  
而在Grunt中，Grunt主要是以文件为媒介来运行它的工作流的，比如在Grunt中执行完一项任务后，会把结果写入到一个临时文件中，然后可以在这个临时文件内容的基础上执行其它任务，执行完成后又把结果写入到临时文件中，然后又以这个为基础继续执行其它任务...就这样反复下去。  
所以，Gulp的速度会比Grunt快。

用法：
```bash
gulp.src(globs[, options])
```
__globs__参数是文件匹配模式(类似正则表达式)，用来匹配文件路径(包括文件名),也可以使指定文件。  

1. `*` 匹配文件路径中的0个或多个字符，但不会匹配路径分隔符，除非路径分隔符出现在末尾
2. `**` 匹配路径中的0个或多个目录及其子目录,需要单独出现，即它左右不能有其他东西了。如果出现在末尾，也能匹配文件。
3. `?` 匹配文件路径中的一个字符(不会匹配路径分隔符)
4. `[...]` 匹配方括号中出现的字符中的任意一个，当方括号中第一个字符为^或!时，则表示不匹配方括号中出现的其他字符中的任意一个，类似js正则表达式中的用法
5. `!(pattern|pattern|pattern)` 匹配任何与括号中给定的任一模式都不匹配的
6. `?(pattern|pattern|pattern)` 匹配括号中给定的任一模式0次或1次，类似于js正则中的`(pattern|pattern|pattern)?`
7. `+(pattern|pattern|pattern)` 匹配括号中给定的任一模式至少1次，类似于js正则中的`(pattern|pattern|pattern)+`
8. `*(pattern|pattern|pattern)` 匹配括号中给定的任一模式0次或多次，类似于js正则中的`(pattern|pattern|pattern)*`
9. `@(pattern|pattern|pattern)` 匹配括号中给定的任一模式1次，类似于js正则中的`(pattern|pattern|pattern)`
10. 展开模式。展开模式以花括号作为定界符，根据它里面的内容，会展开为多个模式，最后匹配的结果为所有展开的模式相加起来得到的结果。如下：
    * `a{b,c}d` 会展开为 `abd`,`acd`  
    * `a{b,}c` 会展开为 `abc`,`ac`  
    * `a{0..3}d` 会展开为 `a0d`,`a1d`,`a2d`,`a3d`  
    * `a{b,c{d,e}f}g` 会展开为 `abg`,`acdfg`,`acefg`  
    * `a{b,c}d{e,f}g` 会展开为 `abdeg`,`acdeg`,`abdeg`,`abdfg`

有多种匹配模式的时候可以使用数组,如：`gulp.src(['js/*.js','css/*.css','*.html'])`

__options__为可选参数,有3个属性buffer、read、base。

*options.buffer*：  类型：`Boolean`  默认：`true` ，设置为`false`，将返回file.content的流并且不缓冲文件，处理大文件时非常有用；  
*options.read*：  类型：`Boolean`  默认：`true` ，设置`false`，将不执行读取文件操作，返回null；  
*options.base*：  类型：`String`  ,设置输出路径以某个路径的某个组成部分为基础向后拼接。

* ## gulp.dest()

gulp.dest()方法是用来写文件的，其语法为：

```bash
gulp.dest(path[,options])
```
__path__:类型： `String` or `Function`  
gulp的流程一般是：先通过`gulp.src()`方法获取到想要处理的文件流，然后将文件流通过`pipe()`方法导入到gulp的插件中，最后把经过插件处理后的流再通过`pipe()`方法导入到`gulp.dest()`中，`gulp.dest()`方法将流中的内容写入到文件中，这里首先需要弄清楚的一点是，我们给`gulp.dest()`传入的路径参数，只能用来指定要生成的文件的目录，而不能指定生成文件的文件名，它生成文件的文件名使用的是导入到它的文件流自身的文件名，所以生成的文件名是由导入到它的文件流决定的，即使我们给它传入一个带有文件名的路径参数，然后它也会把这个文件名当做目录名
```javascript
var gulp = require('gulp');
gulp.src('script/A.js')
    .pipe(gulp.dest('dist/B.js'));
//最终生成的文件路径为 dist/B.js/A.js,而不是dist/B.js
```
`gulp.dest(path)`生成的文件路径是我们传入的path参数后面再加上`gulp.src()`中有通配符开始出现的那部分路径

```javascript
var gulp = reruire('gulp');
//有通配符开始出现的那部分路径为 **/*.js
gulp.src('script/**/*.js')
    .pipe(gulp.dest('dist')); //最后生成的文件路径为 `dist/**/*.js`
//如果 **/*.js 匹配到的文件为 js/A.js ,则生成的文件路径为 `dist/js/A.js`
```

__options__:  
*options.cwd* 类型： `String` 默认值： `process.cwd()` 输出目录的 `cwd` 参数，只在所给的输出目录是相对路径时候有效。  
*options.mode* 类型： `String` 默认值： `0777` 八进制权限字符，用以定义所有在输出目录中所创建的目录的权限。

* ## gulp.task()

gulp.task()方法用来定义任务，其语法为：

```bash
gulp.task(name[, deps], function)
```
__name__ ：任务名称
__deps__ ：是当前定义的任务需要依赖的其他任务，为一个数组。当前定义的任务会在所有依赖的任务执行完毕后才开始执行。如果没有依赖，则可省略这个参数
__function__ ：为任务函数，我们把任务要执行的代码都写在里面。该参数也是可选的。

如果任务相互之间没有依赖，任务会按你书写的顺序来执行，如果有依赖的话则会先执行依赖的任务。  
如果某个任务所依赖的任务是异步的，gulp并不会等待那个所依赖的异步任务完成，而是会接着执行后续的任务。例如：

```javascript
gulp.task('A',function(){
    //A是一个异步执行的任务
    setTimeout(function(){
        console.log('A is done')
    },5000);
});
//B任务虽然依赖于A任务,但并不会等到A任务中的异步操作完成后再执行
gulp.task('B',['A'],function(){
    console.log('B is done');
});
```

如果要等异步任务中的异步操作完成后再执行后续的任务，有三种方法：  
* 在异步操作完成后执行一个回调函数来通知gulp这个异步任务已经完成,这个回调函数就是任务函数的第一个参数

```javascript
gulp.task('A',function(cb){ //cb为任务函数提供的回调，用来通知任务已经完成
    //A是一个异步执行的任务
    setTimeout(function(){
        console.log('A is done');
        cb();  //执行回调，表示这个异步任务已经完成
    },3000);
});

//这时B任务会在A任务中的异步操作完成后再执行
gulp.task('B',['A'],function(){
  console.log('B is done');
});
```

* 定义任务时返回一个流对象。适用于任务就是操作gulp.src获取到的流的情况

```javascript
gulp.task('A',function(cb){
    var stream = gulp.src('js/*.js')
      .pipe(dosomething()) //dosomething()中有某些异步操作
      .pipe(gulp.dest('build'));
    return stream;
});

gulp.task('B',['A'],function(){
    console.log('B is done');
});
```

* 返回一个promise对象

```javascript
var Q = require('q'); //一个著名的异步处理的库 https://github.com/kriskowal/q
gulp.task('one',function(cb){
    var deferred = Q.defer();
    // 做一些异步操作
    setTimeout(function() {
        deferred.resolve();
    }, 5000);
    return deferred.promise;
    });

gulp.task('two',['one'],function(){
    console.log('two is done');
});
```

* ## gulp.watch()

gulp.watch()用来监视文件的变化，当文件发生变化后，我们可以利用它来执行相应的任务，例如文件压缩等。其语法为

```bash
gulp.watch(glob[, options], tasks)
```

__glob__: 为要监视的文件匹配模式，规则和用法与`gulp.src()`方法中的globs相同。
__options__: 用法与`gulp.src()`方法中的options相同
__tasks__: 为文件变化后要执行的任务，为一个数组

```javascript
gulp.task('A',function(){
  //do something
});
gulp.task('B',function(){
  //do something
});
gulp.watch('js/*.js', ['A','B']);
```

gulp.watch()还有另外一种使用方式：

```bash
gulp.watch(globs[, options, cb])
```
globs和options参数与第一种用法相同
cb参数为一个函数。每当监视的文件发生变化时，就会调用这个函数,并且会给它传入一个对象，该对象包含了文件变化的一些信息，type属性为变化的类型，可以是added,changed,deleted；path属性为发生变化的文件的路径.

```javascript
gulp.watch('js/*.js', function(event){
    console.log(event.type); //变化类型 added为新增,deleted为删除，changed为改变 
    console.log(event.path); //变化的文件的路径
}); 
```
