---
title: 不百度就不会用的技巧-JavaScript篇
date: 2019-12-26 18:21:14
categories:
- [技术,JavaScript]
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3EqET.jpg
---
本文罗列了一些工作中经常使用到、但是不查百度就不会用的一些知识点和技巧。
<!-- more -->
1. 数组常用的一些方法
[传送门（js数组常用方法）](https://wanghong.cool/2019/12/26/blog3)

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

4. 深拷贝
var deepCloneObj = JSON.parse(JSON.stringify(obj))



5. 截取字符串或数组
 **slice(起始位置index1,结束位置index2)**
需要注意的是，截取的结果包括index1对应的项，不包括index2对应的项
```javascript
let str = 'sesshouin kiara'
console.log(str.slice(0,5))  //输出"sessh"

let arr = [1,2,3,4,5,6,7]
console.log(arr.slice(0,5))  //输出[1,2,3,4,5]
```



