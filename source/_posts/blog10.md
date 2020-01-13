---
title: 数字保留两位小数
date: 2020-01-08 15:16:59
categories:
- [技术,JavaScript]
tags:
- JavaScript
- Vue
- Ant Design
thumbnail: https://s2.ax1x.com/2020/01/08/lgtuXd.jpg
---
把数字保留两位小数，可以直接使用toFixed。
```javascript
let a = 2.335
let num1 = a.toFixed(2)
console.log(num1)
```
但是 toFixed 并不是真正意义上的四舍五入，百度上很多大神的解释如下：
toFixed() 方法可把 Number 四舍五入为指定小数位数的数字。例如将数据Num保留2位小数，则表示为：toFixed(2)；但是其四舍五入的规则与数学中的规则不同，使用的是**银行家舍入规则**，银行家舍入：所谓银行家舍入法，其实质是一种四舍六入五取偶（又称四舍六入五留双）法。具体规则如下：

简单来说就是：**四舍六入五考虑，五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一**。

显然这种规则不符合我们平常在数据中处理的方式。为了解决这样的问题，可以自定义去使用 Math.round 方法进行自定义式 的实现指定保留多少位数据进行处理。
```javascript
let num2= (Math.round(a * 100) / 100).toFixed(2)
console.log(num2)
```
round() 方法可把一个数字舍入为最接近的整数。例如：Math.round(x)，则是将x取其最接近的整数。其取舍的方法使用的是四舍五入中的方法，符合数学中取舍的规则。对于小数的处理没有那么便捷，但是可以根据不同的要求，进行自定义的处理。

例如：对于X进行保留两位小数的处理，则可以使用Math.round(X * 100) / 100.进行处理。