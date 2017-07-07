---
title: JavaScript算法之数组去重
date: 2016-10-08 16:39:49
categories: study_blog
tags: [JavaScript,算法,前端]
---

看面试题，偶然看到这个题目，然后想着写写代码实现一下。

<!-- more -->

1. 首先想到最简单的方法，就是遍历数组，然后去重，代码如下（不改变原数组）
```javascript
function unique1(arr){
	let newArr = [];
	const arrLen = arr.length;
	for(let i = 0; i < arrLen; i++){
		let mark = false;
		for(let j = i + 1 ; j < arrLen; j++){
			if(arr[i] === arr[j]){
				mark = true;
				break;
			}
		}
		if(!mark){
			newArr.push(arr[i]);
		}
	}
	return newArr;
}
```
2. 第二个办法也是遍历，不过这个办法会改变原来数组，用到了数组的`splice()`方法
```javascript
function unique2(arr) {
	const arrLen = arr.length;
	for (let i = 0; i < arrLen; i++) {
		let mark = false;
		for (let j = i + 1; j < arrLen; j++) {
			if(arr[i] === arr[j]){
				arr.splice(j, 1);
			}
		}
	}
	return arr;
}
```
3. 利用javascript中的`Set`中值唯一，然后`Array.from()`转化成数组
```javascript
function unique3(arr){
	return Array.from(new Set(arr));
}
```
4. 遍历数组，建立新数组，利用indexOf判断是否存在于新数组中，不存在则push到新数组，最后返回新数组
```javascript
function unique4(arr) {
	let ret = [];
	const arrLen = arr.length;
	for (let i = 0; i < arrLen; i++) {
		if (ret.indexOf(arr[i]) === -1) {
			ret.push(arr[i]);
		}
	}
	return ret;
}
```

暂且先写这四个方法，其他应该还有许多奇技淫巧...