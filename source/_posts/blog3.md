---
title: 常用的数组操作方法
date: 2019-12-26 17:01:18
categories:
- [技术,JavaScript]
tags:
- JavaScript
- 常用
thumbnail: ../images/blog3.jpg
---
本文记录了一些工作中经常使用到、但是不查百度就不会用的数组操作方法。
<!-- more -->
#### str和数组的互相转化
* toString()、join()
* split()
```javascript
//数组转字符串
let arr1 = [1,2,3,4]
let str1 = arr1.toString()
let str2 = arr1.join('-')
console.log(str1)  //输出"1,2,3,4"
console.log(str2)  //输出"1-2-3-4"

//字符串转数组
let arr2 = str1.split(",")
let arr3 = str2.split("-")
console.log(arr2)  //输出[1,2,3,4]
console.log(arr3)  //输出[1,2,3,4]
```

#### 栈方法
* push()
添加一项到数组末尾，并返回**修改后数组的长度**
* pop()
从数组末尾移除最后一项，减少数组的 length 值，然后返回**移除的项**
