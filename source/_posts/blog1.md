---
title: js实现引用类型的深拷贝
date: 2019-12-22 15:01:51
categories:
- 技术
- JavaScript
tags:
- JavaScript
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3E5gs.jpg
---
**var deepCloneObj = JSON.parse(JSON.stringify(obj))**
<!-- more -->
### 为什么要使用深拷贝？
我们希望得到一个新的引用类型的变量（数组、对象），并且在修改这个引用类型变量的时候，不改变原数组（对象）的值。

根据不包含引用对象的普通数组深拷贝得到启发，不拷贝引用对象，拷贝一个字符串会开辟一个新的存储地址，这样就切断了引用对象的指针联系。
```javascript
let obj = {
    name: 'Boa Hancock',
    age: 31,
    gender: 'female'
}

var deepCloneObj = JSON.parse(JSON.stringify(obj));//拷贝对象

console.log('obj:',obj)
console.log('deepCloneObj:',deepCloneObj)

```

打印结果如下,两个对象的值是一样的：
<div style="text-align:center;"><img src="/print1.png"></div>


修改新对象的值：
```javascript
deepCloneObj.name = 'Sesshouin Kiara'

console.log('obj:',obj)
console.log('deepCloneObj:',deepCloneObj)

```
打印结果如下：
<div style="text-align:center;"><img src="/print2.png"></div>
新对象的name属性改变了，但是并没有影响到原对象name属性的值，这说明新对象和旧对象指向不同的存储空间，拷贝完成。