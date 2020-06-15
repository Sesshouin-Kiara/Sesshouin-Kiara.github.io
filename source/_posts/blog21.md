---
title: 移动端适配方案：postcss-px-to-viewport
date: 2020-05-09 17:10:58
categories:
- H5
tags:
- H5
- 移动端
thumbnail: https://s2.ax1x.com/2019/12/31/l3E5gs.jpg
---
做移动端的项目时，都会遇到适配相关的问题，例如微信公众号、手机端官网、APP的内嵌H5等。公司采用的解决方案是<span class="backgroundBlock">postcss-px-to-viewport</span>，个人感觉这个方案使用起来超级简单，非常推荐。而且现在浏览器对<span class="backgroundBlock">viewport</span>的支持越来越高，兼容性这方面也不需要担心。
<!-- more -->
顾名思义，<span class="backgroundBlock">postcss-px-to-viewport</span>就是将<span class="backgroundBlock">px</span>单位转化成<span class="backgroundBlock">viewport</span>，来帮你解决不同设备间的适配问题。

安装：
```
$ npm install postcss-px-to-viewport
```

在vue项目的根目录下新建一个配置文件<span class="backgroundBlock">postcss.config.js</span>：
```js postcss.config.js
module.exports = {
  "plugins": {
    "postcss-px-to-viewport": {
      viewportWidth: 750,       // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334,     // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3,         // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
      viewportUnit: 'vw',       // 指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore', '.hairlines','.van-',/^(.van)/,/^(.igno)/], 
                                // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1,         // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false         // 允许在媒体查询中转换`px`
    },
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false
    }
  }
}
```
配置文件的各项参数含义都写在注释中了。
