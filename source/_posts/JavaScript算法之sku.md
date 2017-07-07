---
title: JavaScript算法之sku
date: 2017-6-28 20:04:49
categories: study_blog
tags: [JavaScript,算法,前端]
---

这几天公司出的题目，之前项目中写过一个类似的，但是写的很猥琐（一些表现是通过频繁操作DOM实现的），借着有奖励的机会重写了一下。

<!-- more -->
## sku: 
> SKU=Stock Keeping Unit（库存量单位）。即库存进出计量的基本单元，可以是以件，盒，托盘等为单位。SKU这是对于大型连锁超市DC（配送中心）物流管理的一个必要的方法。现在已经被引申为产品统一编号的简称，每种产品均对应有唯一的SKU号。单品：对一种商品而言，当其品牌、型号、配置、等级、花色、包装容量、单位、生产日期、保质期、用途、价格、产地等属性与其他商品存在不同时，可称为一个单品。

以上摘自[百度百科](http://baike.baidu.com/item/SKU)

## 思路：
通过字典键值对（在JavaScript中即是Object数据类型）的方式来查找对应可选属性。  

## 难点：
在于所确定属性的同级可选属性。  

## 实现步骤：
1. 将拿到的数据重新组织成需要展示的数据格式、计算使用的字典数据格式、以及计算过程中需要的一些辅助数据。
2. 获取页面的所选的属性。
3. 根据所选属性组成查找key来查找结果。
4. 将结果缓存，方便下次加速查找。
5. 表现到页面。
6. 确定商品。

## 核心代码：
```javascript
/**
 * 得到结果
 * @param {string} key 查找关键字以；分割
 * @return {array} 所有可选属性数组
*/
getResult(key, isRealFind = true) {
	// 如缓存中存在，则直接返回结果
	if (this.cacheData[key] && isRealFind) {
		this.result = this.cacheData[key];
		this.resultID = this.goodsDict[key] ? this.goodsDict[key] : '';
		console.log(this.resultID);
		return this.result;
	}

	// 继续查找
	let result = '';
	for (let _key in this.goodsDict) {
		let keyArr = key.split(';');
		let _keyArr = _key.split(';');
		let arr = keyArr.concat(_keyArr);
		arr = Array.from(new Set(arr));
		if (arr.length === _keyArr.length) {
			result += _key;
		}
	}

	if (isRealFind) {
		// 所有可选属性
		this.result = result.split(';');
		let _keyArr = key.split(';');
		if (_keyArr[_keyArr.length - 1] === '') {
			_keyArr.pop();
		}
		for (let i = 0; i < _keyArr.length; i++) {
			let _arr = key.split(';');
			let str = _arr.splice(i, 1);
			let oldResult = this.getResult(_arr.join(';'), false);
			let index = '';
			// 获取该key所在索引
			this.allKeys.forEach((item, i) => {
				if (item.indexOf(str.join('')) !== -1) {
					index = i;
					return;
				}
			});
			this.allKeys[index].forEach(item => {
				if (oldResult.indexOf(item) !== -1) {
					this.result.push(item);
				}
			});
		}
		this.result = Array.from(new Set(this.result));

		// 缓存数据
		this.cacheData[key] = this.result;
		this.resultID = this.goodsDict[key] ? this.goodsDict[key] : '';
		console.log(this.resultID);
		return this.result;
	} else {
		return result;
	}
}
```
## github
[github地址](https://github.com/sniperbo/algorithms/tree/master/src)  (应该拉下来就能跑，页面展示用的jQuery，因为当前项目需要，换成mvvm框架页面表现会更加简单)
