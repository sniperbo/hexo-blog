---
title: JavaScript中的拷贝
date: 2017-3-23 23:11:59
categories: study_blog
tags: [JavaScript, 前端]
---

要说 JavaScript 的拷贝问题，首先要了解的是 JavaScript 的数据类型。  
在 JavaScript 中数据类型大致可以分成三类，分别是基本数据类型、引用数据类型和特殊数据类型。  
<!-- more -->
基本数据类型有：`Number`、`String`、`Boolean`、  
引用数据类型有：`Object`(其中包含`Array`, `Object`, `Date`, `RegExp`)
特殊数据类型：`Null`、`Undefined`  
ES6 引入了一种新的原始数据类型 `Symbol`，表示独一无二的值。所以现在JavaScript中一共有7种数据类型。

其中特殊类型`Undefined`是变量声明了但是没有赋值，`Null`表示一个无的对象是一个`Object`，这两者在判断语句中都会被转成`false`，其实都是表示没有，但为什么要有两个，说起来比较复杂，可以看看阮老师的文章[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)。

继续往下说，下面简单说说基本数据类型和引用数据类型的区别。  
最大的区别就是，基本数据类型的变量标识符和变量值都存在栈内存，而引用类型在栈内存中存储的是变量标识符和指针，然后在堆内存中保存对象内容。如下图：  
![基本类型引用类型存储示意图](http://od4k4xerr.bkt.clouddn.com/2017040301.png)

现在开始说拷贝的问题，基本数据类型，我们直接  
```JavaScript
let a = 1;
let a1 = a;
```
就可以了。而如果是引用类型这样的话  
```JavaScript
let b = {a: 1};
let b1 = b;
```
此时`b`和`b1`的值就会互相影响也就是改变了他们中任何一个值，另外一个值也会相应的改变。  
来个图：  
![基本类型和引用类型的普通拷贝](http://od4k4xerr.bkt.clouddn.com/2017040302.png)

由此可见使用`=`的方式对于基本类型是有效的而对于引用类型是不可以的。

在jQuery中我们可以直接使用jQuery的`extend()`方法来进行深拷贝。

使用原生JavaScript我们应该这么做呢？

如果是`Array`，且数组中的数据都是基本数据类型，我们可以利用数组中的`slice()`方法来返回一个新的数组，像这样：  
```JavaScript
let arrA = [1, 2, 3, 4, 5, 6, 7, 8, 9];
let arrA1 = arrA.slice();
```

如果是`Object`的keyValuePair，且对应的value都是基本数据类型，则可以用[`Object.assign()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)来操作。  
```JavaScript
let a = {a: 1};
let a1 = Object.assign({}, a);
```

上述两种方法，只能在特定的情况下有用，一旦数组的值是引用类型或者对象中value的值是一个引用类型，就不起作用了。如果遇到复杂的数据类型该怎么进行深拷贝呢？以前在不少帖子里看过说使用JSON来做个中转，类似于这样：  
```JavaScript
let a = {a: {a : {a: 1}}};
let a1 = JSON.parse(JSON.stringify(a));
```
这个方法好像很无敌，貌似无论多么复杂的数据类型都能深拷贝，但是一旦遇到key对应的value是Function或者其他JSON不支持的类型就不行了，可以参照stackoverflow上的这个回答[javascript deep copy using JSON](http://stackoverflow.com/questions/20662319/javascript-deep-copy-using-json)

所以在JavaScript中深拷贝是一件非常费事的事情。大部分情况下我们并不需要深拷贝，再仔细看看自己写的函数非得深拷贝不可吗？


恩，如果你非要！

1. 看看jQuery `extend()`方法的具体实现，这里有个参考资料[jQuery源码](https://github.com/jquery/jquery/blob/1472290917f17af05e98007136096784f9051fab/src/core.js#L121)  
2. 使用[lodash的`cloneDeep()`方法](https://lodash.com/docs#cloneDeep)实现比较复杂,当然也比较好  
3. 再贴一个stackoverflow上的回答[How to Deep clone in javascript](http://stackoverflow.com/questions/4459928/how-to-deep-clone-in-javascript)  
```JavaScript
function clone(item) {
	if (!item) { return item; } // null, undefined values check

	var types = [ Number, String, Boolean ], 
		result;

	// normalizing primitives if someone did new String('aaa'), or new Number('444');
	types.forEach(function(type) {
		if (item instanceof type) {
			result = type( item );
		}
	});

	if (typeof result == "undefined") {
		if (Object.prototype.toString.call( item ) === "[object Array]") {
			result = [];
			item.forEach(function(child, index, array) { 
				result[index] = clone( child );
			});
		} else if (typeof item == "object") {
			// testing that this is DOM
			if (item.nodeType && typeof item.cloneNode == "function") {
				var result = item.cloneNode( true );    
			} else if (!item.prototype) { // check that this is a literal
				if (item instanceof Date) {
					result = new Date(item);
				} else {
					// it is an object literal
					result = {};
					for (var i in item) {
						result[i] = clone( item[i] );
					}
				}
			} else {
				// depending what you would like here,
				// just keep the reference, or create new object
				if (false && item.constructor) {
					// would not advice to do that, reason? Read below
					result = new item.constructor();
				} else {
					result = item;
				}
			}
		} else {
			result = item;
		}
	}

	return result;
}
```
4. 自己撸一个...
5. 待我有空撸一个(逃