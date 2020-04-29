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
1. **Date对象转换为指定格式的字符串**，如：<span class="backgroundBlock">YYYY-mm-dd HH:MM</span><span class="backgroundBlock">YYYY-mm-dd</span>等。
```javascript 一个Date对象
let now = new Date()
console.log(now) 
//打印结果：Fri Jan 03 2020 10:05:48 GMT+0800 (中国标准时间)
```
定义格式化Date的方法<span class="backgroundBlock">format</span>：
```javascript
format(date,format){
    let dateFormatStr = ''
    let year = date.getFullYear()
    let mouth = date.getMonth()+1 //Date对象中月份为0-11
    let day = date.getDate()
    let hour = date.getHours()
    let min = date.getMinutes()
    let s = date.getSeconds()
    // 将format格式中的字符串替换
    dateFormatStr = format.replace(/yyyy/, year).replace(/MM/, mouth).replace(/dd/, day).replace(/hh/, hour).replace(/mm/, min).replace(/ss/, s)
    return dateFormatStr
}
```
<!-- more -->
对<span class="backgroundBlock">Date</span>对象调用<span class="backgroundBlock">format</span>方法:
```javascript
let nowStr = this.format(now,'yyyy-MM-dd hh:mm:ss')
console.log(nowStr) 
//打印结果：2020-1-1 10:5:48
```
对 format 方法传入不同的 format 参数，即可转换成不同格式。




2. **字符串转换成Date对象**
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
注意：<span class="importantBlock">Date对象中月份是0-11</span>，所以date4这样传入整数生成的Date对象代表2020年2月1日