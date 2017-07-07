---
title: JavaScript算法之分页(备忘)
date: 2016-11-19 20:05:54
categories: study_blog
tags: [JavaScript,算法,前端,备忘]
---
很久没写东西了，哎...

不能荒废了这博客，还是写点废话吧...

最近开始实习了，然后最近在做一个项目，其中有一个评论的分页搞得我很是头疼，最后写了个小小的算法。

记录下来做备忘吧！！！

不多说，直接上代码：

<!-- more -->

```JavaScript
function pages(nowPage, totalPage){
	var str = nowPage + '';

	for (var i = 1; i <= 3; i++) {
		if (nowPage - i > 1) {
			str = nowPage - i + ' ' + str;
		}
		if (nowPage + i < totalPage) {
			str = str + ' ' + (nowPage + i);
		}
	}

	if (nowPage - 4 > 1) {
		str = '... ' + str;
	}

	if (nowPage > 1) {
		str = 'first ' + 'prev ' + 1 + ' ' + str;
	}

	if (nowPage + 4 < totalPage) {
		str = str + ' ...';
	}

	if (nowPage < totalPage) {
		str = str + ' ' + totalPage + ' next' + ' last';
	}

	return str.split(' ');
}
```

以上这个代码最后会返回一个分页的数组，包含上一页，下一页，首页和尾页。

返回值如下：
```JavaScript
[ 'first', 'prev', '1', '2', '3', '4', '5', '...', '10', 'next', 'last' ]
```

可以取到这些返回值，然后做一些判断渲染到Html页面中...

实习快一个月了，过几天写一篇实习总结，需要好好批判自己一番了...
