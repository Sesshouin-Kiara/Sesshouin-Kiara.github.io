---
title: 一些常用的前端知识点-JavaScript篇（一）
date: 2019-12-26 18:21:14
categories:
- [技术,JavaScript]
tags:
- JavaScript
- 常用
thumbnail: ../images/blog2.jpg
---
本文罗列了一些工作中经常使用到、但是不查百度就不会用的一些知识点和技巧。
<!-- more -->
1. 数组常用的一些方法
[传送门（js数组常用方法）](https://sesshouin-kiara.github.io/2019/12/26/blog3)

2. 字符串去空格
```javascript
let str1 = "   aaa   "
str1.trim()
console.log(str1) //trim()不会改变原字符串，打印结果str1为"   aaa   "

let str2 = str1.trim()
console.log(str2) //trim()会返回一个新的字符串，打印结果str2为"aaa"
```

3. 遍历js对象
```javascript
let obj = {
    name: 'Boa Hancock',
    age: 31,
    gender: 'female'
}
// 方法一
for(var name in obj) {
     console.log(name,":",obj[name]);
}
// 方法二
Object.keys(obj).forEach(function(key){
     console.log(key,":",obj[key]);
});
/*
打印结果：
name : Boa Hancock
age : 31
gender : female
*/
```

5. 深拷贝
var deepCloneObj = JSON.parse(JSON.stringify(obj))



