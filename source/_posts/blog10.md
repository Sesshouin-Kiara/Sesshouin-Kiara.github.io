---
title: 数字保留两位小数及toFixed的bug带来的思考
date: 2020-01-08 15:16:59
categories:
- JavaScript
tags:
- JavaScript
thumbnail: https://s2.ax1x.com/2020/01/08/lgtuXd.jpg
---
把数字保留两位小数，可以直接使用<span class="backgroundBlock">toFixed</span>。
```javascript
let a = 2.335
let num1 = a.toFixed(2)
console.log(num1)
```
但是<span class="backgroundBlock">toFixed</span>并不是真正意义上的四舍五入，很多大神的解释如下：
<!-- more -->
>toFixed() 方法可把 Number 四舍五入为指定小数位数的数字。例如将数据Num保留2位小数，则表示为：toFixed(2)；但是其四舍五入的规则与数学中的规则不同，使用的是**银行家舍入规则**，银行家舍入：所谓银行家舍入法，其实质是一种四舍六入五取偶（又称四舍六入五留双）法。具体规则如下：

简单来说就是：**四舍六入五考虑，五后非零就进一，五后为零看奇偶，五前为偶应舍去，五前为奇要进一**。
不过，**实际上我自己的测试结果与上述规则不符**，有时候是五前为偶进一，五前为奇舍去，不知道哪里的问题（可以F12打开 开发者调试工具进行测试）。例如：
* 2.305.toFixed(2) = 2.31 
不符合，偶进一
* 2.315.toFixed(2) = 2.31  
不符合，奇舍去
* 2.325.toFixed(2) = 2.33  
不符合，偶进一
* 2.335.toFixed(2) = 2.33  
不符合，奇舍去
* 2.345.toFixed(2) = 2.35 
不符合，偶进一
* 2.355.toFixed(2) = 2.35  
不符合，奇舍去
* 2.365.toFixed(2) = 2.37  
不符合，偶进一
* 2.375.toFixed(2) = 2.38  
符合，奇进一
* 2.385.toFixed(2) = 2.38  
符合，奇舍去
* 2.395.toFixed(2) = 2.40  
符合，奇进一

2.3开头时会出现这种异常状况，2.4/2.5开头又正常了，2.6/2.7/2.8又不正常了，等等等等。  

百度搜索“银行家舍入算法”、“toFixed”之类的关键词，甚至还有很大一部分搜索靠前的帖子博文中的 toFixed 算出来的结果都是错的（即我用F12调试得出来的结果和作者在文中写的结果不一致），让我怀疑他们是不是复制粘贴，根本没有试过结果的正确与否。  

我不知道为什么会出现这种问题，首先我感觉是不是我对银行家舍入规则理解错了，不过后来仔细看了一下银行家舍入规则，发现并没有错误，确实是五前为偶应舍去，五前为奇要进一。

那难道是 toFixed 其实并没有使用银行家舍入规则？我看了下 W3school 和 MDN Web Docs，都只是很简洁的说了句 toFixed 功能是四舍五入，并没有深入提及其他的东西。

从结果来看，toFixed很明显不是四舍五入，也不是四舍六入五考虑，那么 toFixed 到底采用的是什么算法呢？  

突然想起以前也遇到过一些number类型的问题，最后发现是js的精度导致的，那么这里会不会也是精度的问题呢？  

这是之前不符合银行家舍入规则的两组数据：
* 2.305.toFixed(2) = 2.31 
不符合，偶进一
* 2.315.toFixed(2) = 2.31  
不符合，奇舍去

将两组数据的精度提升到20位，得到如下结果：
* 2.305.toFixed(20) = 2.30500000000000015987 
* 2.315.toFixed(20) = 2.31499999999999994671

果然出现问题了，可以发现，将2.305精度提高后，发现得到了一个略大于2.305的值，而2.315得到了一个略小于2.315的值。

提升精度后，值却改变了，这说明原先的我输入的2.305本身就不是真正意义上的2.305，而是一个略大的近似值，可以这样证明（在开发者工具中进行调试）：
1. 打印<span class="backgroundBlock">2.toFixed(20)</span>，输出<span class="backgroundBlock">2.00000000000000000000</span>，说明2的值至少20位精度时是精确的。
2. 打印<span class="backgroundBlock">2.305-2</span>，输出<span class="backgroundBlock">0.30500000000000016</span>，该值大于0.305，说明输入的2.305确实是一个比真正的2.305大的数字。

可以得出结论了：
> toFixed 采用的确实是银行家舍入规则，但是由于js精度带来的误差，导致我们看到了一些不符合银行家舍入规则的数据。

至于为何会出现精度问题，这就涉及到一些底层知识了，本文不做探讨，可以参考[这篇文章](https://zhuanlan.zhihu.com/p/31202697)。

不过，不管是四舍六入五考虑，还是精度带来的误差，都不符合我们一般情况下数据处理的方式，即四舍五入。为了解决这样的问题，可以使用<span class="backgroundBlock">Math.round</span>方法实现指定保留任意位数据。
```javascript
let num2= (Math.round(a * 100) / 100).toFixed(2)
```
<span class="backgroundBlock">Math.round()</span>方法可把一个数字舍入为最接近的整数。其取舍的方法使用的是<span class="importantBlock">四舍五入</span>，符合数学中取舍的规则。对于小数的处理没有那么便捷，但是可以根据不同的要求，进行自定义的处理。

例如：对于X进行保留两位小数的处理，则可以使用<span class="backgroundBlock">Math.round(X*100)/100</span>进行处理。
* 2.355.toFixed(2) = 2.35
* Math.round(2.355 * 100) / 100 = 2.36

有时候原数据位数不够，需要补齐位数，可以<span class="importantBlock">round 和 toFixed一起使用</span>：
* (Math.round(1.5 * 100) / 100).toFixed(2) = 1.50
<div style="height:12px;"></div>

### <span class="importantBlock">终极解决方案</span>如下：
**(Math.round(<span class="backgroundBlock">number</span>*<span class="backgroundBlock">10<sup>digit</sup></span>)/<span class="backgroundBlock">10<sup>digit</sup></span>).toFixed(<span class="backgroundBlock">digit</span>)**
>number：需要进行舍入操作的数据
digit：保留的位数