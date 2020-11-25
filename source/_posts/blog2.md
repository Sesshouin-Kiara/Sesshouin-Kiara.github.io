---
title: 盘点一些常用的语法和知识点-JavaScript篇
date: 2019-12-26 18:21:14
categories:
- JavaScript
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
（持续更新中......）
本文盘点了一些工作中会经常使用到的，或者是比较重要但是不太熟练的语法和知识点。

1. 数组常用的一些方法
[传送门（js数组常用方法）](https://wanghong.cool/2019/12/26/blog3)

2. 字符串去空格
```javascript trim()
let str1 = "   aaa   "
str1.trim()
console.log(str1) //trim()不会改变原字符串，打印结果str1为"   aaa   "

let str2 = str1.trim()
console.log(str2) //trim()会返回一个新的字符串，打印结果str2为"aaa"
```
<!-- more -->

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


6. 实现函数的重载
<div class="myhr"></div>
我们知道，js是没有函数重载这一概念的。因为函数实际上就是对象，声明两个同名函数，结果只会是后面的函数覆盖了前面的函数（<span class="whdiv2">覆盖引用</span>）。
<div class="myhr"></div>
那么，我们怎么来模拟函数的重载功能吗？答案是<span class="whdiv2">函数的参数</span><span class="whdiv1">arguments</span>。
<div class="myhr"></div>
arguments 对象 与 数组 类似，可以使用 方括号语法，通过 arguments 可以访问函数接收到的<span class="whdiv2">实参数组</span>。

```js
function addFunc() {
     if(arguments.length === 1){
          return arguments[0]+1
     }else if(arguments.length === 2){
          return arguments[0]+arguments[1]
     }else if(arguments.length >= 3){
          return '超出计算范围'
     }
}

addFunc(1)  // 2
addFunc(5,6)  // 11
addFunc(1,2,3)  // '超出计算范围'
```


