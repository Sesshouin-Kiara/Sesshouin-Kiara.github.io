---
title: 常用的js数组方法
date: 2019-12-26 17:01:18
categories:
- JavaScript
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
本文系统的整理了工作中比较常用的数组方法。

#### 一、数组检测方法
* **instanceof**
* **Array.isArray()（推荐）**
```javascript
let arr = [1,2,3,4];
console.log(arr instanceof Array); // true 
console.log(Array.isArray(arr)); // true 
```
<span class="whdiv1">instanceof</span>在跨多个 iframe（即多个全局执行环境）时会失效，<span class="whdiv1">Array.isArray()</span>才是最终的解决办法。
<!-- more -->
<div class="myhr"></div>

#### 二、数组和字符串的互相转化
* **toString()、toLocaleString()、join()**
* **split()**
```javascript
//数组转字符串
let baseArr = [1,2,3,4]
let str1 = baseArr.toString()
let str2 = baseArr.join('-')
console.log(str1)  //输出"1,2,3,4"
console.log(str2)  //输出"1-2-3-4"

//字符串转数组
let arr1 = str1.split(",")
let arr2 = str2.split("-")
console.log(arr1)  //输出[1,2,3,4]
console.log(arr2)  //输出[1,2,3,4]
```
<div class="myhr"></div>

#### 三、数组操作方法（增、删、拼接、裁剪、替换等）
* **push()**
添加若干项到数组末尾，并返回<span class="whdiv2">修改后数组的长度</span>。
* **pop()**
从数组末尾移除一项，减少数组的 length 值，然后返回<span class="whdiv2">移除的项（最后一项）</span>。
* **unshift()**
添加若干项到数组前端，并返回<span class="whdiv2">修改后数组的长度</span>。
* **shift()**
从数组前端移除一项，减少数组的 length 值，然后返回<span class="whdiv2">移除的项（第一项）</span>。<div class="myhr"></div>
* **concat()**
拼接数组，concat()的参数既可以接收一个数组，也可以接收非数组。concat()<span class="whdiv2">不会改变原数组，但是会返回拼接过的新数组</span>
```javascript
let arr1 = [1,2,3]; 
let arr2 = [4,5]; 

let arr3 = arr1.concat(arr2)
let arr4 = arr1.concat('w','h')

console.log(arr3)   //打印结果：[1, 2, 3, 4, 5]
console.log(arr4)   //打印结果：[1, 2, 3, "w", "h"]
```

* **slice()**
裁剪数组。该方法接收1或2个参数，即起始和结束位置，返回一个新的数组（注意：新的数组<span class="whdiv2">包括起始位置的项，不包括结束位置的项</span>。slice()方法也<span class="whdiv2">不会改变原数组</span>）。在只传递一个参数的情况下，默认返回从起始位置到数组末尾的所有项。
```javascript
let arr1 = [1,2,3,4,5]; 

let arr2 = arr1.slice(0,3) 
let arr3 = arr1.slice(2) 

console.log(arr2)   //打印结果：[1, 2, 3],返回数组不含索引为3的值
console.log(arr3)   //打印结果：[3,4,5]
```

* **splice()**
splice()功能强大，可以实现<span class="whdiv2">增、删、改</span>功能，接收的参数如下：
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
<div class="myhr"></div>

#### 四、位置方法
* **indexOf()**
* **lastIndexOf()**
```javascript
let arr = [1,2,3,4,5,4,3,2,1]; 
console.log(arr.indexOf(4)); //输出3，只会返回第一个值为4的项的位置
console.log(arr.lastIndexOf(2)); //输出7,不是1。虽然是从数组末尾开始计算位置，但也是从index=0开始查找的
console.log(arr.indexOf(99)); //输出-1
```
<span class="whdiv1">arr.indexOf('item')===-1</span>常用来<span class="whdiv2">判断数组中是否存在某值</span>
<div class="myhr"></div>

#### 五、重排序方法
* **reverse()**（<span class="whdiv2">翻转</span>）
* **sort()** （<span class="whdiv2">排序，默认按字符串顺序升序</span>）
```javascript
let arr = [1, 2, 3, 4, 5]; 
arr.reverse();
console.log(arr) //输出[5, 4, 3, 2, 1]
```
<span class="whdiv1">reverse()</span>方法会改变原数组。
```javascript
let arr = [5, 0, 1, 15, 10]; 
arr.sort(); 
console.log(arr); //0,1,10,15,5
```
<span class="whdiv1">sort()</span>方法也会改变原数组，默认情况下先调用每项的<span class="whdiv1">toString</span>，然后比较得到的字符串，所以10排在5前面。<div class="whhr"></div>
这种排序方式在很多情况下都不是最佳方案。因此<span class="whdiv1">sort()</span>方法可以<span class="whdiv2">接收一个比较函数</span>作为参数，以便我们指定排序的规则。以下就是一个简单的比较函数：
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
<div class="myhr"></div>

#### 六、迭代方法
* **forEach()**
对数组中的每一项运行给定函数。这个方法没有返回值。
* **map()**
对数组中的每一项运行给定函数，返回<span class="whdiv2">每次函数调用的结果组成的数组</span>。
* **filter()**
对数组中的每一项运行给定函数，返回<span class="whdiv2">该函数会返回 true 的项组成的数组</span>。
* **every()**
对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。 
* **some()**
对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。<div class="whhr"></div>
这些方法都接收3个参数，分别是<span class="whdiv2">数组该项的值，该项的索引，原数组</span>
```javascript map示例1
let arr = [1,2,3]
let arr2 = arr.map((val,index,arr) => {
    return val*2
})
console.log(arr2)   // 输出 [2, 4, 6]
```
```javascript map示例2
let foodList = [
    {
        name:'小白菜',
        cate:'蔬菜',
        nutList:{
            calorie:3,
            fat:0,
            vitamin:20,
        }
    },
    {
        name:'牛肉',
        cate:'肉类',
        nutList:{
            calorie:38,
            fat:12,
            vitamin:9,
        }
    }
]
let foodList2 = foodList.map((t)=>({...t,...t.nutList}))
console.log(foodList2)  
// 输出结果可在f12在开发者工具中自行赋值代码运行查看哦
```
```javascript filter示例1
let personList = [
    {
        name:'luffy',
        age:18
    },
    {
        name:'nami',
        age:22
    },
    {
        name:'hancock',
        age:28
    }
]
let personList2 = personList.filter((t) => {
    return t.age>18
})
console.log(personList2)   
// 输出 [{name: "nami", age: 22},{name: "hancock", age: 28}]
```

