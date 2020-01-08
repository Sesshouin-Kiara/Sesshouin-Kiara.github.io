---
title: 不百度就不会用的技巧-HTML/CSS篇
date: 2019-12-30 14:19:04
categories:
- [技术,HTML]
- [技术,CSS]
tags:
- HTML
- CSS
- 常用
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
本文罗列了一些工作中经常使用到、但是不查百度就不会用的一些知识点和技巧。

#### 一、Flex 布局
内容转自[阮一峰的Flex 布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html),不过个人觉得阮一峰老师教程页面的布局和色调不是很利于阅读，于是在这里重构一下。详细内容还是建议看原文。
```css
.box{
  display: flex;
}
```
常用属性
* flex-direction
决定主轴的方向（即项目的排列方向）。
```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
<!-- more -->
* flex-wrap
如果一条轴线排不下，如何换行。默认情况下，不换行
```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
* justify-content
定义项目在主轴上的对齐方式。
```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
* align-items
定义项目在交叉轴上如何对齐。
```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```
* align-content
定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

#### 二、嘿嘿嘿
