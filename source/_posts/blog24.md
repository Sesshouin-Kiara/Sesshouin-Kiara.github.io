---
title: 前端面试题汇总
date: 2020-06-19 14:57:37
categories:
- HTML
- CSS
tags:
- HTML
- CSS
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
本文整理了一些前端面试经常会涉及到的知识点。

**1.语义化标签**

例如<span class="backgroundBlock">\<title\></span><span class="backgroundBlock">\<header\></span><span class="backgroundBlock">\<footer\></span><span class="backgroundBlock">\<strong\></span><span class="backgroundBlock">\<article\></span>等。

作用：提高代码的可读性和开发效率，以及让网络爬虫更好的解析。
<!-- more -->
<br>

**2.移动端适配**
```html
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```
<br>

**3.盒模型**

盒模型分为两种：<span class="importantBlock">标准盒模型</span>和<span class="importantBlock">怪异盒模型</span>，使用<span class="backgroundBlock">box-sizing</span>来定义：
```css
box-sizing: content-box;    //标准盒模型
box-sizing: border-box;     //怪异盒模型
```
标准盒模型的<span class="backgroundBlock">width / height</span>指的是<span class="importantBlock">content</span>的宽度/高度：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog24_img1.jpg"></div>
怪异盒模型的<span class="backgroundBlock">width / height</span>指的是<span class="importantBlock">content + padding + border</span>的宽度/高度：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog24_img2.jpg"></div>
<br>
<br>

**4.元素水平和垂直居中**

（1）让元素内的文本或图片<span class="backgroundBlock">水平居中</span>，使用:<span class="importantBlock">text-align: center;</span>  

（2）让元素内的文本<span class="backgroundBlock">垂直居中</span>，让其<span class="importantBlock">line-height 和 height 相等</span>

（3）让父元素内的子元素<span class="backgroundBlock">水平垂直居中</span>，采用<span class="importantBlock">绝对定位、外边距auto</span>的方法：
```css 父元素
.boxWrap {
    /* 添加以下属性 */
    position: relative;
}
```
```css 子元素
.box {
    /* 添加以下属性 */
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
}
```
>该方法的局限性是必须知道div的确定宽高。

（4）让父元素内的子元素<span class="backgroundBlock">水平垂直居中</span>，使用<span class="importantBlock">flex</span>，这个算是<span class="importantBlock">最普适</span>的方法：
```css 给父元素添加以下css属性：
.boxWrap {
    display: flex;
    justify-content: center;
    align-items: center;
}
```
<br>

**5.localstorage、sessionStorage和cookie的比较**
1. **大小**
sessionStorage：5M或者更大
localStorage：5M或者更大
cookie：一般不超过4K

2. **数据有效期**
sessionStorage：仅在当前<span class="importantBlock">浏览器窗口关闭之前有效</span>，关闭页面或者浏览器会被清除
localStorage：<span class="importantBlock">永久有效</span>，窗口或者浏览器关闭也会一直保存，除非手动永久清除，因此用作持久数据
cookie：一般由<span class="importantBlock">服务器生成，可以设置失效时间</span>；若没有设置时间，关闭浏览器cookie失效，若设置了时间，cookie就会存放在硬盘里，过期才失效

3. **作用域**
sessionStorage：不可以在不同的浏览器中访问、<span class="importantBlock">不可以在不同的tab页中访问</span>、必须同源
localStorage：不可以在不同的浏览器中访问、<span class="importantBlock">可以在不同的tab页中访问</span>、必须同源
cookie：不可以在不同的浏览器中访问、可以在不同的tab页中访问、必须同源
<br>

**6.position各种定位的区别**

（1）<span class="backgroundBlock">static</span>：默认值。没有定位，元素出现在正常的流中

（2）<span class="backgroundBlock">relative</span>：相对定位，相对于<span class="importantBlock">原位置</span>进行定位，不脱离文档流。

（3）<span class="backgroundBlock">absolute</span>：绝对定位，相对于<span class="importantBlock">最近一级的定位不是 static 的父元素</span>来进行定位，脱离文档流。

（4）<span class="backgroundBlock">fixed</span>: 固定定位的元素，相对于<span class="importantBlock">浏览器窗口或 frame</span>进行定位，脱离文档流。
<br>

**7.css选择器及其权重**

这里列出所有样式的权重，不仅仅是选择器：

（1）<span class="importantBlock">! important</span>，权重<span class="importantBlock">最高</span>

（2）<span class="importantBlock">内联</span>样式，权重<span class="importantBlock">1000</span>

（3）<span class="importantBlock">id</span>选择器，权重<span class="importantBlock">100</span>

（4）<span class="importantBlock">类、伪类、属性</span>选择器，权重<span class="importantBlock">10</span>

（5）<span class="importantBlock">元素、伪元素</span>选择器，权重<span class="importantBlock">1</span>

（6）<span class="importantBlock">选择器组合</span>，权重<span class="importantBlock">会相加，但不能越级</span>

（7）<span class="importantBlock">继承</span>的样式，<span class="importantBlock">没有权重</span>

<br>

**8.网络状态码**

HTTP状态码分类：

（1）<span class="backgroundBlock">1**</span>：<span class="importantBlock">信息</span>，服务器收到请求，需要请求者继续执行操作

（2）<span class="backgroundBlock">2**</span>：<span class="importantBlock">成功</span>，操作被成功接收并处理

（3）<span class="backgroundBlock">3**</span>：<span class="importantBlock">重定向</span>，需要进一步的操作以完成请求

（4）<span class="backgroundBlock">4**</span>：<span class="importantBlock">客户端错误</span>，请求包含语法错误或者无法完成请求

（5）<span class="backgroundBlock">5**</span>：<span class="importantBlock">服务器错误</span>，服务器在处理请求的过程中发生了错误

常见的HTTP状态码：

* 200 —— OK，请求成功

* 301 —— Moved Permanently，资源（网页等）被永久转移到其他URL

* 302 —— Found，307——Temporary Redirect，临时重定向，请求的文档被临时移动到别处

* 304 —— Not Modified，未修改，表示客户端缓存的版本是最近的

* 401 —— Unauthorized，请求要求用户的身份认证

* 403 —— Forbidden，禁止，服务器理解客户端请求，但是拒绝处理此请求，通常是权限设置所致

* 404 —— Not Found，请求的资源（网页等）不存在

* 500 —— Internal Server Error——内部服务器错误

* 502 —— Bad Gateway，充当网关或代理的服务器从远端服务器接收到了一个无效的请求

* 504 —— Gateway Time-out，充当网关或代理的服务器，未及时从远端服务器获取请求
<br>

**9.一次完整的HTTP事务流程有哪些步骤？**

（1）域名解析

（2）发起TCP的三次握手

（3）建立TCP连接后发起http请求

（4）服务器响应http请求，浏览器得到HTML代码

（5）浏览器解析HTML代码，并请求HTML代码中的资源

（6）浏览器对页面进行渲染呈现给用户

（7）连接结束
<br>

**10.性能优化方案**

（1）减少 HTTP请求数

（2）资源合并与压缩（js、css等，减少资源的大小）

（3）Css Sprite(雪碧图、精灵图)<图像拼合技术>

（4）使用字体图标来代替图片

（5）优化资源加载：CSS文件放在head中，JS文件放在body底部，先外链，后本页

（6）优化网络连接，使用CDN加速

（7）模块按需加载

（8）使用缓存
<br>

**其他之前做过的知识点汇总**：

[js篇](https://wanghong.cool/2019/12/26/blog2/)

[html/css篇](https://wanghong.cool/2019/12/30/blog4/)

[es6/7篇](https://wanghong.cool/2020/06/17/blog23/)

[vue-router篇](https://wanghong.cool/2020/03/14/blog14/)

[vuex](https://wanghong.cool/2020/03/17/blog15/)

[ajax](https://wanghong.cool/2020/03/18/blog16/)

[闭包](https://wanghong.cool/2020/03/20/blog18/)
