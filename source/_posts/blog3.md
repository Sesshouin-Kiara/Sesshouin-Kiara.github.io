---
title: 常用的js数组方法
date: 2019-12-26 17:01:18
categories:
- [技术,JavaScript]
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
本文系统的整理了工作中比较常用的数组方法。

#### 一、数组检测方法
* instanceof
* Array.isArray()**（推荐）**
```javascript
let arr = [1,2,3,4];
console.log(arr instanceof Array); // true 
console.log(Array.isArray(arr)); // true 
```
instanceof在跨多个iframe（即多个全局执行环境）时会失效，Array.isArray()才是最终的解决办法。
<!-- more -->


#### 二、数组操作方法（增删等）
* push()
添加若干项到数组末尾，并返回**修改后数组的长度**。
* pop()
从数组末尾移除一项，减少数组的 length 值，然后返回**移除的项（最后一项）**。
* unshift()
添加若干项到数组前端，并返回**修改后数组的长度**。
* shift()
从数组前端移除一项，减少数组的 length 值，然后返回**移除的项（第一项）**。
* concat()
拼接数组，concat()的参数既可以接收一个数组，也可以接收非数组
```javascript
let arr1 = ["red", "green", "blue"]; 
let arr2 = colors.concat("yellow", ["black", "brown"]);
console.log(arr2)  //打印结果：["red", "green", "blue", "yellow", "black", "brown"]
console.log(arr1)  //打印结果：["red", "green", "blue"]
```
concat()**不会改变原数组**，但是会返回拼接过的新数组
* splice()
splice()功能强大，可以实现增、删、改功能，接收的参数如下：
```javascript
splice(index,num,newItem1,newItem2,......)
//index：起始位置（数组索引）
//num：要删除的项数
//newItem1,newItem2,......：要插入的项

//举例
let arr = [1,2,3]
//增
arr.splice(1,0,'haha')
console.log(arr) //打印结果：[1, "haha", 2, 3]
//删
arr.splice(1,1)
console.log(arr) //打印结果：[1, 2, 3]
//改
arr.splice(1,1,'heihei')
console.log(arr) //打印结果：[1, 'heihei', 3]
```


#### 三、str和数组的互相转化
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



#### 四、位置方法
* indexOf()
* lastIndexOf()
```javascript
let arr = [1,2,3,4,5,4,3,2,1]; 
console.log(arr.indexOf(4)); //输出3，只会返回第一个值为4的项的位置
console.log(arr.lastIndexOf(2)); //输出7,不是1。虽然是从数组末尾开始计算位置，但也是从index=0开始查找的
console.log(arr.indexOf(99)); //输出-1
```
arr.indexOf('item')===-1常用来判断数组中是否存在某值


#### 五、重排序方法
* reverse()（翻转）
* sort() （排序，默认按字符串顺序升序）
```javascript
let arr = [1, 2, 3, 4, 5]; 
arr.reverse();
console.log(arr) //输出[5, 4, 3, 2, 1]
```
reverse()方法会改变原数组。
```javascript
let arr = [5, 0, 1, 15, 10]; 
arr.sort(); 
console.log(arr); //0,1,10,15,5
```
sort()方法也会改变原数组，默认情况下先调用每项的toString，然后比较得到的字符串，所以10排在5前面。
这种排序方式在很多情况下都不是最佳方案。因此 sort()方法可以接收一个比较函数作为参数，以便我们指定排序的规则。以下就是一个简单的比较函数：
```javascript
/*
比较函数接收两个参数，
如果第一个参数应该位于第二个之前则返回一个负数，
如果两个参数相等则返回 0，
如果第一个参数应该位于第二个之后则返回一个正数。
*/
function compare(value1, value2) { 
    if (value1 < value2) { 
        return -1; 
    } else if (value1 > value2) { 
        return 1; 
    } else { 
        return 0; 
    } 
}

let arr = [5, 0, 1, 15, 10]; 
arr.sort(compare);
console.log(arr); //0,1,5,10,15
```


#### 五、迭代方法
* every()
对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。 
* filter()
对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
* forEach()
对数组中的每一项运行给定函数。这个方法没有返回值。
* map()
对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
* some()
对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。

这些方法都接收3个参数，分别是 *数组该项的值，该项的索引，原数组*
```javascript
let arr = [1,2,3]
arr.forEach((val,index,arr) => {
    // 需要进行的操作
})
```
以上方法都不会修改数组中的包含的值，但可以自行在回调函数中写入操作数组的代码。

