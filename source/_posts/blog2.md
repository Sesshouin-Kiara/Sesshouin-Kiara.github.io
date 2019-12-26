---
title: 一些常用的前端知识点-JavaScript篇（一）
date: 2019-12-26 16:21:14
categories:
- [技术,JavaScript]
tags:
- JavaScript
- 常用
thumbnail: ../images/blog2.jpg
---
本文罗列了一些工作中经常使用到、但是不查百度就不会用的一些知识点和技巧。
<!-- more -->
### JavaScript
1. 字符串去空格
```javascript
let str1 = "   aaa   "
str1.trim()
console.log(str1) //trim()不会改变原字符串，打印结果str1为"   aaa   "

let str2 = str1.trim()
console.log(str2) //trim()会返回一个新的字符串，打印结果str2为"aaa"
```


2. 拼接数组
```javascript
let arr1 = [1,2]
let arr2 = [3,4]
arr1.concat(arr2)
console.log(arr1) //concat()不会改变原数组，打印结果arr1为[1,2]

let arr3 = arr1.concat(arr2)
console.log(arr3) //concat()会返回一个新数组，打印结果arr3为[1,2,3,4]
```


3. 数组常用的一些方法
[js数组常用方法-传送门](https://sesshouin-kiara.github.io/2019/12/26/blog3/#more)


4. 遍历js对象和数组



