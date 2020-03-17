---
title: js日期对象Date和字符串的互相转化
date: 2020-01-01 10:03:11
categories:
- JavaScript
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3E5gs.jpg
---
1. 将js的Date对象转换为指定格式，如：YYYY-mm-dd HH:MM、YYYY-mm-dd等。
```javascript
let now = new Date()
console.log(now) 
//打印结果：Fri Jan 03 2020 10:05:48 GMT+0800 (中国标准时间)
```
定义格式化Date的方法：
```javascript
format(data,format){
    let dataFormatStr = ''
    let year = data.getFullYear()
    let mouth = data.getMonth()+1 //Date对象中月份为0-11
    let day = data.getDate()
    let hour = data.getHours()
    let min = data.getMinutes()
    let s = data.getSeconds()
    // 将format格式中的字符串替换
    dataFormatStr = format.replace(/yyyy/, year).replace(/MM/, mouth).replace(/dd/, day).replace(/hh/, hour).replace(/mm/, min).replace(/ss/, s)
    return dataFormatStr
}
```
<!-- more -->
对now调用format方法
```javascript
let nowStr = this.format(now,'yyyy-MM-dd hh:mm:ss')
console.log(nowStr) 
//打印结果：2020-1-1 10:5:48
```
对format方法传入不同的format参数，即可转换成不同格式。




2. 将字符串转换成Date对象：
```javascript
//传入字符串
let date1 = new Date("2020-1-1")
let date2 = new Date("2020/1/1")
let date3 = new Date("2020,1,1")
//传入整数参数
let date4 = new Date(2020,1,1)

console.log(date3)
//Wed Jan 01 2020 00:00:00 GMT+0800 (中国标准时间)
console.log(date4)
//Sat Feb 01 2020 00:00:00 GMT+0800 (中国标准时间)
```
注意：Date对象中月份是0-11，所以date4这样传入整数生成的Date对象代表2020年2月1日