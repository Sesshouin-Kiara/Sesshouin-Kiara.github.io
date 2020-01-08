---
title: 从零开始使用Vue Router
date: 2020-01-06 10:26:07
categories:
- [技术,JavaScript]
- [技术,Vue]
tags:
- JavaScript
- 常用
- Vue
- Vue Router
thumbnail: https://s2.ax1x.com/2020/01/08/lgtuXd.jpg
---
公司项目大都是在已经搭建好的模板上进行开发，加上自己也没有认真梳理过，导致工作大半年了对vue-router还是不太熟练，每次使用都要看文档。这次趁着有时间，在这里系统地梳理一下vue相关内容，从零开始。

首先创建一个vue项目，然后安装vue-router。  

NPM
```
npm install vue-router
```
Yarn
```
yarn add vue-router
```
建议使用Yarn，安装速度更快更稳定。npm速度慢而且经常下载下来缺失文件导致项目运行失败。
<!-- more -->
## 开始使用
首先在src/components中新建foo.vue和bar.vue两个文件，作为路由显示的页面内容。
然后在src下面新建一个router.js文件，写入以下内容：
```javascript
// 引入Vue和VueRouter
import Vue from 'vue'
import VueRouter from 'vue-router'

// 引入（路由）组件
import Foo from './components/foo.vue'
import Bar from './components/bar.vue'

// 告诉 Vue 使用 VueRouter
Vue.use(VueRouter)

// 定义路由router
const router = new VueRouter({
    routes: [
        {
            path: '/foo',
            component: Foo
        },
        {
            path: '/bar',
            component: Bar
        },
    ]
});

// 将router暴露出去供其他文件使用
export default router;
```
此时项目的目录结构为：
<div style="text-align:center;"><img src="https://s2.ax1x.com/2020/01/06/lrIv4g.png"></div>

在main.js中引入router.js文件：
```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
    router,// 将路由注入到根实例中
    render: h => h(App),
}).$mount('#app')
```

在App.vue中添加router-link和router-view：
```html
<template>
    <div id="app">
        <h1>Hello VueRouter!</h1>
        <div>
            <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
            <router-link to="/foo">Go to Foo</router-link>
            <router-link to="/bar">Go to Bar</router-link>
        </div>
         <!-- 路由出口 -->
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name: 'app',
        components: {
        }
    }
</script>
```
```css
<style>
    #app {
        text-align: center;
        margin-top: 60px;
    }
    a {
        margin-right: 16px;
        text-decoration: none;
    }
</style>
```

运行项目，效果如下，点击页面上的router-link可以切换显示对应的router组件：
<div style="text-align:center;"><img src="https://s2.ax1x.com/2020/01/06/lrbzXd.png"></div>
<div style="text-align:center;"><img src="https://s2.ax1x.com/2020/01/06/lrqpnA.png"></div>

但是有一个问题，当首次进入页面的时候，router-view中并没有显示任何内容。这是因为首次进入页面时，它的路径是 '/'，我们并没有给这个路径做相应的配置。
我们可以在router.js中添加一项路由，给空路径'/'指定一个组件，
```javascript
{
    path: '/',
    component: Foo
}
```
或者是给空路径'/'重定向一个其他的路径，（建议使用这种方式）
```javascript
{
    path: '/',
    component: Foo
}
```

修改之后router.js内容如下：
```javascript
// 引入Vue和VueRouter
import Vue from 'vue'
import VueRouter from 'vue-router'

// 引入（路由）组件
import Foo from './components/foo.vue'
import Bar from './components/bar.vue'

// 告诉 Vue 使用 VueRouter
Vue.use(VueRouter)

// 定义路由router
const router = new VueRouter({
    routes: [
        // {
        //     path: '/',
        //     component: Foo
        // },
        {
            path: '/',
            redirect: '/foo'
        },
        {
            path: '/foo',
            component: Foo
        },
        {
            path: '/bar',
            component: Bar
        },
    ]
});

// 将router暴露出去供其他文件使用
export default router;
```

## 嵌套路由
App.vue中的 router-view 是最顶层的出口，渲染最高级路由匹配到的组件。同样的，一个被渲染组件同样可以包含自己的嵌套 router-view 。例如，在 Bar 组件的模板添加一个 router-view，其中包含 BarChild1 和 BarChild2 两个子组件。
在 src/components 下面新建 bar-child1.vue 和 bar-child2.vue 两个文件，然后将 bar.vue 和 router.js 修改成如下内容：
```html
<!-- bar.vue -->
<template>
    <div style="margin:33px;">
        这里是bar
        <div>
            <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
            <router-link :to="{name:'BarChild1'}">Go to bar-child1</router-link>
            <router-link :to="{name:'BarChild2'}">Go to bar-child2</router-link>
        </div>
        <!-- 路由出口 -->
        <router-view></router-view>
    </div>
</template>
```
```javascript
// router.js
import Vue from 'vue'
import VueRouter from 'vue-router'

import Foo from './components/foo.vue'
import Bar from './components/bar.vue'

import BarChild1 from './components/bar-child1.vue'
import BarChild2 from './components/bar-child2.vue'

Vue.use(VueRouter)

const router = new VueRouter({
    routes: [{
            path: '/',
            redirect: '/foo'
        },
        {
            path: '/foo',
            component: Foo,
            name: 'Foo'
        },
        {
            path: '/bar',
            component: Bar,
            name: 'Bar',
            children: [{
                    path: 'bar-child1',
                    component: BarChild1,
                    name: 'BarChild1',
                },
                {
                    path: 'bar-child2',
                    component: BarChild2,
                    name: 'BarChild2',
                },
            ]
        },
    ]
});

export default router;
```
为了方便使用，我们在定义路由的时候加上了name字段，在router-link中，我们可以使用对象来代替之前的路径了，注意这时候的to前面要加":"。  
运行效果如下：
<div style="text-align:center;"><img src="https://s2.ax1x.com/2020/01/06/lsGMlt.png"></div>

**要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**
例如，当router.js中BarChild1组件的路由path是这样时：
```javascript
{
    path: 'bar-child1',
    component: BarChild1,
    name: 'BarChild1',
}
```
BarChild1在浏览器地址栏的路径是：localhost:8080/#/bar/bar-child1。  

而当router.js中BarChild1组件的路由path是以 / 开头时：
```javascript
{
    path: '/bar-child1',
    component: BarChild1,
    name: 'BarChild1',
}
```
BarChild1在浏览器地址栏的路径是：localhost:8080/#/bar-child1

## 路由元信息
工作中，我们常常还会为路由定义一些自定义属性，这些属性我们可以放在meta字段（路由元信息）中：
```javascript
{
    path: '/home',
    component: Home,
    name: 'Home',
    meta: {
        name: '主页',
        isShow: true,
        requiresAuth: true,
        ......
    }
},
```
可以通过this.$router.meta[属性名]拿到对应的值。

## 编程式的导航
除了通过 router-link 来创建导航链接外，我们还可以借助 router 的实例方法，通过js代码来实现。
将App.vue中的router-link修改为button，通过添加点击事件方法同样可以完成路由的切换。
```html
<template>
    <div id="app">
        <h1>Hello VueRouter!</h1>
        <div>
            <router-link to="/foo">Go to Foo</router-link>
            <button @click="goPage('Bar')">Go to Bar</button>
        </div>
        <!-- 路由出口 -->
        <router-view></router-view>
    </div>
</template>
```
```javascript
export default {
    name: 'app',
    components: {
    },
    methods: {
        goPage(routerName) {
            this.$router.push({
                name: routerName
            })
        }
    }
}
```
效果如下：
<div style="text-align:center;"><img src="https://s2.ax1x.com/2020/01/06/lsNk3d.png"></div>



Vue Router的基本使用就是这样了，更加复杂的使用可以看：[使用Vue搭建后台管理系统](https://www.baidu.com/)。