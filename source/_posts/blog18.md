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
讨论js闭包的文章，网络上多不胜数。但是要加深理解，还是要亲自撸一遍。
### 1. 如何产生闭包？（产生闭包的条件）
**函数嵌套时，内部(子)函数引用了外部(父)函数的变量(函数)，并且调用了外部(父)函数，此时就会产生闭包。**
<!-- more -->
```html 闭包的一个例子
<html>
<head>
    <meta charset="utf-8">
    <title>Page Title</title>
</head>
<body>
    <script type="text/javascript">
        function fn1() {
            let name = "Jack"
            let age = 28
            function fn2() {
                console.log('my name is ' + name)
            }
            fn2()
        }
        fn1()//调用fn1
    </script>
</body>
</html>
```

### 2. 闭包是什么？
> 对于闭包是什么，很多种理解：
> 1. 嵌套的内部函数
> 2. 包含被引用的变量的对象（存在于内部函数中）
> 3. 内部函数以及它能访问到的变量

对于这些闭包是什么的解释，可能并不是能直观，也不方便初学者的理解。所以我们通过Chrome的开发者工具来看看。  

还是上面那段代码，在<span class="backgroundBlock">fn2()</span>那一行打上断点，调试查看。此时的上下文变量如图：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog18_img1.png"></div>

<span class="backgroundBlock">fn1</span>函数的内部环境中，有<span class="backgroundBlock">name、age、fn2</span>3个变量。

点开<span class="backgroundBlock">fn2</span>，可以发现其中有个<span class="backgroundBlock">Closure</span>（闭包）对象：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog18_img2.png"></div>

需要注意到的是，<span class="backgroundBlock">Closure</span>对象内部只有<span class="backgroundBlock">name</span>属性，并没有<span class="backgroundBlock">age</span>，这是因为<span class="backgroundBlock">fn2</span>中并没有引用<span class="backgroundBlock">age</span>属性。

通过开发者工具，就能比较直观的理解闭包的3种说法了。

我们可以把闭包理解成<span class="importantBlock">内部函数fn2本身</span>，也可以理解成<span class="importantBlock">fn2中的Closure对象</span>：{name: "Jack"}，或者说是<span class="importantBlock">fn2以及它能访问到的属性（name），即内部函数所处的环境</span>。  


### 3.闭包的特点及作用
>闭包有如下特点：
>1. 可以读取函数内部的变量。
>2. 可以让这些局部变量始终保持在内存中，不会因外部函数调用完毕而被销毁（也就是说，闭包可以使它的诞生环境一直存在）。

因为这两个特点，闭包可以用来<span class="importantBlock">封装私有变量</span>。

### 4. 闭包在实际开发中的运用
上述闭包在实际开发中是没有什么实际意义的，而实际开发中，一般会将内部函数作为外部函数的返回值。 

>注意：闭包的产生与是否返回内部函数无关。

我们来看一个例子：
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
            // this.addStar 现在是一个闭包,8的作用是初始化star
            this.addStar = this.starRepo(8); 
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
在18行，<span class="backgroundBlock">this.starRepo(8)</span>执行会返回一个内部函数（闭包），并将这个内部函数赋给<span class="backgroundBlock">this.addStar</span>。

这里产生了一个闭包，之后每次点击“升2星”按钮时，会执行这个已经产生的内部函数（闭包），但不会产生新的闭包。即无论点击多少次按钮，上述代码都只会在<span class="backgroundBlock">created</span>生命周期中调用外部函数<span class="backgroundBlock">starRepo</span>的时候产生一个闭包。

虽然<span class="backgroundBlock">addStar</span>是在<span class="backgroundBlock">starRepo</span>函数外部调用的，但由于闭包的特性，使得<span class="backgroundBlock">addStar</span>还是能访问<span class="backgroundBlock">starRepo</span>函数内部属性<span class="backgroundBlock">star</span>。  

连续点击5次按钮，输出如下：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog18_img3.png"></div>

函数本来在执行之后会立即销毁，但由于生成了闭包，<span class="backgroundBlock">starRepo</span>中的<span class="backgroundBlock">star</span>始终保存在内存之中，并没有销毁，而是随着每次事件的触发而累加。
<div style="height:12px;"></div>

### 4.结尾

其实上面阐述的对闭包的3种理解，更多的只是为了说出一个知识点，而不是一定要给闭包找到一个绝对正确的定义。

很多时候，一种工具或概念被发明出来，其目的是为了更方便的开发。所以当你觉得某个概念复杂而难以使用时，那很可能是走入了误区。

看过这样一段话，觉得很有道理：

**编程界崇尚以简洁优雅为美，很多时候**

**如果你觉得一个概念很复杂，那么很可能是你理解错了。**
  
<div style="height:24px"></div>

相关链接：

[JS 中的闭包是什么？（来源：知乎）](https://zhuanlan.zhihu.com/p/22486908)

[Javascript闭包——懂不懂由你，反正我是懂了（来源：博客园）](https://kb.cnblogs.com/page/110782/)