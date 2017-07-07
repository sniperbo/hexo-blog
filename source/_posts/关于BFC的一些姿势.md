---
title: 关于BFC的一些姿势
date: 2016-10-08 22:09:18
categories: study_blog
tags: [css,前端]
---
这两天看一些关于面试的题目，老是会看到**BFC**这个词。作为一个野路子页面仔，对于这类的名词到底是什么还是懵逼的。  
那这个**BFC**到底是个什么呢？经过一番查阅发现原来就是这个东西...

<!-- more -->

**BFC**全称是"Block Formating Context"，[官方解释](https://www.w3.org/TR/CSS21/visuren.html#block-formatting)，翻译翻译就是块级格式话上下文，再翻译翻译就是按照块级盒子布局。  
一个页面是由很多个**盒子** 组成的,不同类型的**盒子**,会参与不同的 Formatting Context（决定如何渲染文档的容器）,因此**盒子**内的元素会以不同的方式渲染,这就是说*BFC内部的元素和外部的元素不会互相影响*。


## 首先看看BFC的形成条件

1. float的值不为none；
2. position的值不为static或relative；
3. display的值是inline-block、table-cell、table-caption、flex、inline-flex；
4. overflow的值不为visible。

以上四个条件**只要满足**一个即可（多个也可）。

## BFC中的盒子的布局模式

在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。  
w3c规范如下：
> 在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。浮动也是如此（尽管盒子里的行盒子 Line Box 可能由于浮动而变窄），除非盒子创建了一个新的BFC（在这种情况下盒子本身可能由于浮动而变窄）。


## BFC的一些作用

### 外边距折叠
在常规的流布局中，两盒子的距离由外边距`margin`决定，一般遵循以下规则：
1. 两个相邻的外边距都是正数的时候，外边距取二者较大的；
2. 两个相邻的外边距都是负数的时候，外边距取二者绝对值较大的；
3. 两个相邻的外边距一正一负的时候，取二值之和。

而如果两个盒子在不同的BFC中他们的外边距就不会折叠

### 清除浮动
常规的浮动元素会脱离文档流，就不会撑开父元素（父元素没有设置height），这时可以用BFC来清除浮动，常见的清除浮动方法`overflow: auto;`就是这个道理。  
同理，文字环绕也可以这么解决。

### 多列布局
以下代码可以轻松实现一个两列布局，左边定宽，右边自动占满。

css:
```css
.sidebar{
	background: #10ae58;
	float: left;
	width:180px;
}
.main{
	background: #3d3d3d;
	overflow:hidden;
}
```
html:
```html
<div class="container">
	<div class="sidebar">
		<p>侧边内容</p>
	</div>
	<div class="main">
		<p>主要内容</p>
	</div>
</div>
```

三列四列更多列实现方法也是类似。

## 写在最后
其实在之前写代码的过程中一直有在用**BFC**的相关姿势，只是自己不知道而已。  
哎，还是自己的基础知识太薄弱。  
还有以上文章内容纯属自己的个人片面理解，如有不同见解欢迎留下意见。

...

停电了，网也断了，坑爹的学校啊，还得手机开热点上传这篇...



