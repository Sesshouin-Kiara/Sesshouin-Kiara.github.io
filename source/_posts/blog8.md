---
title: js实现根据sku关键属性算出所有sku的算法
date: 2020-01-08 11:06:06
categories:
- [技术,JavaScript]
tags:
- JavaScript
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
最近在公司做了一个商品中台的项目，商品模块中一个比较基础的部分是根据sku的关键属性算出所有sku组合的需求，在这里记录一下当时用到的算法：

首先我们可以拿到后端返回的sku关键属性
```javascript
let skuKey = [
    {
        name:"颜色",
        value:["黑色","白色"]
    },
    {
        name:"内存",
        value:["64g","128g","256g"]
    },
]
```
现在我们需要根据这个skuKey生成sku组合。
<!-- more -->
先把这个skuKey处理成一个二维数组的格式：
```javascript
let twoDimensionalArray = []
forEach((item,index) => {
    twoDimensionalArray.push(item.value)
})
// twoDimensionalArray = [
//     ["黑色","白色"],
//     ["64g","128g","256g"],
// ];
```

求组合：

```javascript 
function cartesianProductOf() {
    return Array.prototype.reduce.call(arguments,function(a, b) {
        var ret = [];
        a.forEach(function(a) {
            b.forEach(function(b) {
                ret.push(a.concat([b]));
            });
        });
        return ret;
    }, [[]]);
}
 
let combination =cartesianProductOf(...twoDimensionalArray )
console.log(combination)
```
combination结果为：
```javascript 
[
    ["黑色", "64g"],
    ["黑色", "128g"],
    ["黑色", "256g"],
    ["白色", "64g"],
    ["白色", "128g"],
    ["白色", "256g"],
]
```
<!-- 这里有个问题，虽然获取到了所有sku的组合，但是combination只剩下了属性值，诸如“颜色”和“内存”属性名已经没了，这对我们后续的操作会带来不便，所以我们还需要优化算法，保留属性名： -->
