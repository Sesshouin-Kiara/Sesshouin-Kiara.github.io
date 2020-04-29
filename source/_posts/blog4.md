---
title: 不百度就不会用的技巧-HTML/CSS篇
date: 2019-12-30 14:19:04
categories:
- [HTML]
- [CSS]
tags:
- HTML
- CSS
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
（持续更新中......）
本文罗列了一些工作中经常使用到、但是不查百度就不会用的一些知识点和技巧。

#### 一、Flex 布局
内容转自[阮一峰的Flex 布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html),不过个人觉得阮一峰老师教程页面的布局和色调不是很利于阅读，于是在这里重构一下。详细内容还是建议看原文。
```css
.box{
  display: flex;
}
```
常用属性
* **flex-direction**
决定主轴的方向（即项目的排列方向）。
```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
<!-- more -->
<div style="text-align:center;"><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png"></div>

* **flex-wrap**
如果一条轴线排不下，如何换行。默认情况下，不换行
```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
<div style="text-align:center;"><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071006.png"></div>

* **justify-content**
定义项目在主轴上的对齐方式。
```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
<div style="text-align:center;"><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png"></div>

* **align-items**
定义项目在交叉轴上如何对齐。
```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```
<div style="text-align:center;"><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png"></div>

* **align-content**
定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
<div style="text-align:center;"><img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png"></div>


#### 二、子元素选择器：**nth-child()**

1. 选择<span class="importantBlock">第n个</span>子元素，n为数字
<span class="backgroundBlock">:nth-child(n)</span>

2. 选择列表中的<span class="importantBlock">偶数</span>子元素
<span class="backgroundBlock">:nth-child(2n)</span>

3. 选择列表中的<span class="importantBlock">奇数</span>子元素
<span class="backgroundBlock">:nth-child(2n-1)</span>

4. 选择<span class="importantBlock">前n个</span>子元素
【负方向范围】选择第1个到第6个
<span class="backgroundBlock">:nth-child(-n+6){}</span>

5. <span class="importantBlock">从第n个</span>子元素开始选择
【正方向范围】选择从第6个开始的，直到最后
<span class="backgroundBlock">:nth-child(n+6){}</span>

6. 两者结合使用，可以限制选择<span class="importantBlock">某个范围内</span>的子元素
【限制范围】选择第6个到第9个，取两者的交集
<span class="backgroundBlock">:nth-child(-n+9):nth-child(n+6){}</span>

7. 选择列表中的<span class="importantBlock">倒数第n个</span>子元素
<span class="backgroundBlock">:nth-last-child(n)</span>

例子：
```css
/*给class="tagWrap"的div的前三个.tag子元素加上右外边距 */
.tagWrap {
    .tag:nth-child(-n + 3) {
        margin-right: 12px;
    }
}
```
