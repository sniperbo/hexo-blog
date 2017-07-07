---
title: JavaScript算法之二分查找法
date: 2016-12-03 22:42:04
categories: study_blog
tags: [JavaScript,算法,前端]
---
> 二分查找又称折半查找，优点是比较次数少，查找速度快，平均性能好；其缺点是要求待查表为有序表，且插入删除困难。因此，折半查找方法适用于不经常变动而查找频繁的有序列表。首先，假设表中元素是按升序排列，将表中间位置记录的关键字与查找关键字比较，如果两者相等，则查找成功；否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表。重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。
<!-- more -->

以上摘自[百度百科](http://baike.baidu.com/link?url=FfiOkDmX6CKgmHxiNGspTG6aLL3aXYpFGYdgz8GTUhacQBO8V2_BPSdrISRiULnz4A6UU5Q2mz9a1oOQ3pkkc2OuvSzEMapIRSh-rzGyEc7_lZx4Z6DmG541CLHsvxD60e_FR_AxlsMVlqRwbmk9wavPvjbtgUNL-m5zuL-gxKpSAui6fgkUv06e5xlr5GpM)

使用二分查找法的前提是对数据进行排序，然后采用分治的策略进行查找。

## 实现步骤

1. 对数据集进行排序。
2. 将n个元素分成个数大致相同的两半，取a[n/2]与欲查找的x作比较。
3. 如果x=a[n/2]则找到x，算法终止。如 果x小于a[n/2]，继续数组的左半部继续搜索x（这里假设数组元素呈升序排列）。如果x>a[n/2]，则在数组a的右半部继续搜索x。

## 代码

```javascript
function binarySearch(arr, key){
    //对arr进行排序
    arr = arr.sort(function(a, b){
        return a <= b ? -1 : 1 ;
    });

    function indexOf(a, k){
        var start = 0;
        var end = a.length - 1;
        while(start <= end){
            var mid = Math.floor((end-start)/2) + start;
            if(k < a[mid]){
                end = mid - 1;
            }else if(k > a[mid]){
                start = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }
    return indexOf(arr, key);
}
```
 
