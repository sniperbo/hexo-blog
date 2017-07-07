---
title: JavaScript算法之快速排序
date: 2016-10-05 21:27:17
categories: study_blog
tags: [JavaScript,算法,前端]
---
## 介绍

> 快速排序由C. A. R. Hoare在1962年提出。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

<!-- more -->

以上摘自[百度百科](http://baike.baidu.com/link?url=VUy9SLKbR6ELnmQo2CDRBN0elaF0MTtmEjA1l7blXyVXeclRrDXa3jTJVZP48CQulI5pvI0akmXFrvGJgovGM9p2uqeLq53sh6VxLaIq_1XhbKbL_G0OmZ0sUfTrwBD25gxgY1PDUUTnTlJJaHsW_nHw_gsP4-R04QFgZQpvSATyN35hEYHYfBRWl8KLhi87yGNwodIbC-jvvGsqRBTtIWWL9Gceyd7ZMvL_hQe4RNk99vii_5ZCx_05fYpMkYJ1)
 
稳定性 _不稳定_  
时间复杂度 _最好_ `O(nlogn)`  
时间复杂度 _最差_ `O(n^2)`

快速排序是对冒泡排序的一种本质改进。它的基本思想是通过一趟扫描后，使得排序序列的长度能大幅度地减少。在冒泡排序中，一次扫描只能确保最大数值的数移到正确位置，而待排序序列的长度可能只减少1。快速排序通过一趟扫描，就能确保某个数（以它为基准点吧）的左边各数都比它小，右边各数都比它大。然后又用同样的方法处理它左右两边的数，直到基准点的左右只有一个元素为止。

## 实现步骤

1. 在数据集之中，选择一个元素作为"基准"。
2. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
3. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

## 代码

```javascript
function quickSort(arr){
    var left = [];
    var right = [];
    var keyIndex = parseInt(arr.length / 2);
    var key = arr[keyIndex];
    for(var i = 0; i < arr.length; i++){
        if(arr[i] <= key){
            left.push(arr[i]);
        }else if(arr[i] > key){
            right.push(arr[i]);
        }
    }

    return left.concat(right);
}
```