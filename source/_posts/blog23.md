---
title: 常用es6/7语法汇总
date: 2020-06-17 14:13:02
categories:
- es6/7
tags:
- es6/7
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
本文整理了一些常用的es6、es7语法。

**1. let和const**
>es5没有块级作用域的概念，es6新增了块级作用域的概念。let和const只能在<span class="importantBlock">块级作用域</span>里访问，不能跨块访问。

其中，<span class="backgroundBlock">let</span>定义变量，<span class="backgroundBlock">const</span>定义常量（使用时必须初始化，即必须赋值，且不能修改）。
<!-- more -->
<br/>

**2. 模板字符串（\`\`）**
```js 以下a和b是一样的：
const name = 'Boa'
var a = 'my name is '+ name
var b = `my name is ${name}`
```
<br/>

**3. 解构赋值**
```js 数组
var [a,b] = [11,22]
console.log(a,b)   //11 22
```
```js 对象（不完全解构）
var {name,age} = {name:'tom',age:22,id:'430181xxxxxxxx'} //不完全解构
console.log(name,age)   //tom 22
```
<br/>

**4. 扩展运算符（...）**
其功能是将一个数组转为<span class="importantBlock">用逗号分隔的参数序列</span>。
```js
console.log(...[1, 2])  // 1 2 
console.log(1, ...[2, 3, 4], 5)  // 1 2 3 4 5 
```
<br/>

**5. 箭头函数**
>注：箭头函数this永远指向其<span class="importantBlock">父作用域</span>
```js
// 基本形式如下：
var func = (a,b) => {
    return a+b
}
// 当参数只有一个时，可省略参数的括号:
var func = a => {
    return a+1
}
// 箭头后直接接表达式（省略大括号和return），代表直接返回箭头后面的表达式:
var func = a => a+1
```
<br/>

**6. 对象简洁写法**
```js
let name = 'Boa'
let obj = {
    // name: name  //es5写法，必须写完整的key-value键值对
    name,          //es6写法，当键值对前后相等时，可缩写。
    age:32
}
```
<br/>

**7. Promise**
>Promise的含义：
>* Promise 的英文本意是承诺。古人云：“君子一诺千金”，这种“承诺将来会执行”的对象在 JavaScript 中称为 Promise 对象。
>* 它是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，并且这个事件提供统一的 API ，可供进一步处理。

<span class="backgroundBlock">Promise</span>出现之前，代码往往会有过多的回调或者嵌套，导致可读性差、耦合度高、扩展性低，甚至出现<span class="importantBlock">回调地狱</span>。  

<span class="backgroundBlock">Promise</span>就是<span class="importantBlock">用同步编程的方式来编写异步代码</span>，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。

<span class="backgroundBlock">Promise</span>机制的出现，扁平化了代码的结构，大大提高了代码可读性；

```js 一个promise对象
new Promise(
  function (resolve, reject) {
    // 一段耗时的异步操作
    resolve('成功') // 数据处理完成
    // reject('失败') // 数据处理出错
  }
).then(
  (res) => {console.log(res)},  // 成功
  (err) => {console.log(err)}   // 失败
)
```
promise对象有<span class="importantBlock">三个状态</span>：

1. <span class="backgroundBlock">pending</span>初始状态，表示【进行中】；
2. <span class="backgroundBlock">fulfilled</span>【已成功】；
3. <span class="backgroundBlock">rejected</span>【已失败】

>resolve 作用是，将Promise对象的状态从【进行中】变为【已成功】（即从 pending 变为 fulfilled），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

>reject 作用是，将Promise对象的状态从【进行中】变为【已失败】（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

<span class="importantBlock">当promise状态发生改变，就会触发then()里的响应函数处理后续步骤；</span>

>Promise对象的状态改变，只有两种可能：
>1. 从 pending 变为 fulfilled
>2. 从 pending 变为 rejected

这两种情况只要发生，状态就凝固了。也就是说，<span class="importantBlock">promise状态一经改变，就不会再变。</span>

相关文档：[阮一峰ES6之promise](https://es6.ruanyifeng.com/#docs/promise)


<br/>

**8. async/await**

<span class="backgroundBlock">async</span>函数是<span class="backgroundBlock">Generator</span>函数的语法糖。

我们先来看看<span class="backgroundBlock">async</span>函数是怎么处理他的返回值的：
```js
async function testAsync() {
    return "hello async";
}
const result = testAsync();
console.log(result);
```
输出结果如图：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog23_img1.png"></div>

>注：有时候认为resolved和fulfilled是等价的表述，甚至用resolved表示fulfilled。

得出结论：<span class="importantBlock">async函数返回的是一个Promise对象</span>。

如果在 async 函数中 return 一个直接量，async 会把这个直接量通过<span class="backgroundBlock">Promise.resolve()</span>封装成Promise对象。

再来看一个实际运用<span class="backgroundBlock">async/await</span>例子：
```html html
<template>
    <div class="navMain">
        <button @click="print">打印</button>
    </div>
</template>
```
```js js
<script>
export default {
    data() {
        return {
            state: "幼虫"
        };
    },
    methods: {
        grow() {
            return new Promise((resolve, reject) => {
                // 模拟一段耗时的异步操作
                setTimeout(() => {
                    let resData = "成虫";
                    resolve(resData); // 状态从pending变为fulfilled，并将resData作为参数传到then中
                }, 1000);
            }).then(
                res => {
                    console.log(res);
                    this.state = res
                }, // 成功
                err => {
                    console.log(err);
                } // 失败
            );
        },
        async print() {
            await this.grow();
            console.log("现在的状态是：", this.state);
        },
    }
};
</script>
```
点击按钮，控制台输出字符串："现在的状态是：成虫"。

**分析：**
grow函数：
1. <span class="backgroundBlock">grow</span>函数包含一个耗时的异步操作，可以理解为实际开发中的向后台请求接口数据。该函数返回一个<span class="backgroundBlock">promise</span>对象，通过<span class="backgroundBlock">then</span>添加回调函数。

2. 可以看到，这个<span class="backgroundBlock">promise</span>对象一直在等待异步操作（定时器）执行，定时器触发之后通过<span class="backgroundBlock">resolve</span>将<span class="backgroundBlock">promise</span>对象的状态从<span class="backgroundBlock">pending</span>变为<span class="backgroundBlock">fulfilled</span>，从而触发<span class="backgroundBlock">then</span>中的成功回调，并将<span class="backgroundBlock">resData</span>作为参数传到<span class="backgroundBlock">then</span>中。

按照我们的需求，<span class="backgroundBlock">print</span>函数是依赖<span class="backgroundBlock">grow</span>函数执行结果的，也就是说需要等<span class="backgroundBlock">grow</span>执行完毕后再执行自己的代码。

所以我们使用<span class="backgroundBlock">async/await</span>：
><span class="importantBlock">当 async 函数（即 print 函数）执行的时候，一旦遇到了 await ，并且 await 的是一个 Promise 对象，就会阻塞后面的代码，等待这个 Promise 对象 resolve 。异步操作完成后，再接着执行函数体内后面的语句。</span>

相关文档：[理解 JavaScript 的 async/await](https://segmentfault.com/a/1190000007535316)

<br/>