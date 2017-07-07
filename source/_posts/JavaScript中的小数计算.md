---
title: JavaScript中的小数计算
date: 2016-10-07 16:17:59
categories: study_blog
tags: [JavaScript,前端]
---

我们知道在javascript中小数运算是会出现错误的，例如：

```javascript
console.log(0.1 + 0.2);
```

的计算结果并不是`0.3`，而是`0.30000000000000004`。

<!-- more -->

这就很奇怪了，先从`javascript`中的`number`类型说起吧。

在`javascript`中`number`类型就是是浮点数，而在`javascript`中浮点数是IEEE-754 格式规定的，也就是通过二进制数表示的。用二进制书可以准确的表示`1/2`、`1/4`、`1/8`这类的小数，而表示`0.1`这样的小数就会出现问题。  
使用二进制表示`0.1`会被表示成`0.0001 1001 1001 1001…`,其中`1001`会无限循环。  
使用二进制表示`0.2`会被表示成`0.0011 0011 0011 0011…`,其中`0011`会无限循环。  
在`javascript`中双精度浮点数的小数部分最多支持 52 位，并且计算会先将数字转换成二进制计算出结果，再转换成十进制数。
所以`0.1`和`0.2`两者相加之后得到`0.0100110011001100110011001100110011001100...`因浮点数小数位的限制而截断的二进制数字，这时候，再把它转换为十进制，就成了 `0.30000000000000004`。

解决方法：将小数转换成整型数再计算，代码如下：

```javascript
function add(num1, num2) {
	let temp1, temp2, multiple;
	//分别获取两个小数的小数长度
	temp1 = (num1.toString()).split('.')[1].length;
	temp2 = (num2.toString()).split('.')[1].length;

	//通过比较两个数字的小数位获取大的，计算出需要扩大的倍数
	multiple = Math.pow(10, Math.max(temp1, temp1));

	//计算结果
	return (num1 * multiple + num2 * multiple) / multiple;
}

console.log(add(0.1, 0.2)) //0.3
```
其余的减法和乘除也是一样。