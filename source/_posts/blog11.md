---
title: 文字超过部分显示省略号
date: 2020-03-05 15:34:41
categories:
- CSS
tags:
- CSS
thumbnail: https://s2.ax1x.com/2019/12/31/l3E5gs.jpg
---
有两种情况，**超过一行**和**超过多行**时剩余部分显示省略号，有以下两个例子：
1. 标题超过一行时，超过宽度的部分用省略号代替
2. 文本超过（若干）行时，超过这几行的部分用省略号代替
<!-- more -->

**情况一**：写一个招聘模块时遇到了如下问题，职位名称太长以至于影响了页面布局，这时需要隐藏职位名称超出的部分。
修改之前:
<div style="text-align:center;"><img src="/print2.png"></div>
```css
.jobName {
    width: 50%;
}
```
修改之后:
<div style="text-align:center;"><img src="/print1.png"></div>
```css
.jobName {
    width: 50%;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
```
  


**情况二**：审批意见超过两行时，多余部分隐藏并显示省略号和展开按钮，点击可查看全部。
效果如图：
<div style="display:flex;">
<img src="/print3.png">
<div style="width:10px;"></div>
<img src="/print4.png">
</div>
文本不做任何处理，自然展开时的css：

```css
.content {
    font-size: 24px;
    line-height: 40px;
    color: rgba(0, 0, 0, 0.65);
    width: 410px;
}
```

文本收起时，在content类基础上再添加一个content_hidden类
```css
.content_hidden {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
}
```