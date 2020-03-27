---
title: 对Js闭包的理解
date: 2020-03-20 16:39:49
categories:
- JavaScript
tags:
- JavaScript
- 闭包
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
首先，我们通过一个例子来了解闭包是怎么起作用的。
<!-- more -->
```html
<template>
    <div>
        <div>Closure Test</div>
        <a-button @click="addStar(2)">升2星</a-button>   <!-- 2的作用是star每次加2 -->        
    </div>
</template>

<script>
    export default {
        data() {
            return {
                addStar: {}  // 函数也是对象
            };
        },

        created() {
            this.addStar = this.starRepo(8); // addStar 现在是一个闭包,8的作用是初始化star
        },

        methods: { 
            //x:基础星级；y:上升梯度
            starRepo(x) {
                var star = x;
                return function (y) {
                    star = star + y
                    console.log("星级star:",star)
                }
            },
        }
    }
</script>
```
虽然<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">addStar</span>不直接处于<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">starRepo</span>的内部作用域，但<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">addStar</span>还是能访问<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">x</span>和<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">star</span>。  

连续点击5次按钮，输出如下：
<div style="text-align:center;"><img src="/print1.png"></div>
<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">starRepo</span>中的<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">x</span>和<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">star</span>始终保存在内存之中，并没有销毁。
<div style="height:24px;"></div>

**编程界崇尚以简洁优雅为美，很多时候**


**如果你觉得一个概念很复杂，那么很可能是你理解错了。**
  
<div style="height:24px"></div>

相关链接：

[JS 中的闭包是什么？（来源：知乎）](https://zhuanlan.zhihu.com/p/22486908)

[Javascript闭包——懂不懂由你，反正我是懂了（来源：博客园）](https://kb.cnblogs.com/page/110782/)