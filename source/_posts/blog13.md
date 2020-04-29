---
title: 从零开始搭建后台管理系统（一）准备工作
date: 2020-03-12 16:25:19
categories:
- Ant Design
- [Vue,Vue Router]
tags:
- Vue
- Ant Design
- Vue Router
- sass
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
---
本项目基于 [Vue全家桶](https://cn.vuejs.org/v2/guide/) 和 [Ant Design](https://www.antdv.com/docs/vue/use-with-vue-cli-cn/) 进行开发。
>目录
>1. **安装脚手架工具（vue-cli）、创建新项目**
>2. **引入 Ant Design**
>3. **引入 Vue Router**
>4. **引入 node-sass/sass-loader**
>5. **登录页**
>6. 完成页面的基本布局（ 根据路由渲染菜单组件、面包屑组件等 ）
>7. 使用 Vuex 完成菜单的显示隐藏
>8. Ajax 以及 Axios 的封装
>9. 全局方法 Utils、混合 Mixins
>10. 后续功能开发
<!-- more -->

>tips：
>1. npm下载速度慢可使用 cnpm 或 yarn。
>2. 安装各种插件过程中项目可能会报缺少一些依赖之类的错误，安装这些依赖包后仍然报错，大概率是node包版本的问题，建议百度。
>3. eslint有点烦，不想配置的话新建项目的时候 eslint 选择 no

1. **创建项目**
```sh 安装脚手架工具
$ npm install -g @vue/cli
# OR
$ yarn global add @vue/cli
```
```sh 创建新项目
vue create antd-demo
```
2. **引入Ant Design**
```sh 安装antd
$ npm i --save ant-design-vue
```
按需引入等操作移步[Ant Design of Vue 快速上手 文档](https://www.antdv.com/docs/vue/getting-started-cn/)查看
  

3. **安装路由（[从零开始使用Vue Router](https://wanghong.cool/2020/01/06/blog7/)）**
安装路由之前，我们可以在src目录下新建<span class="backgroundBlock">views</span>和<span class="backgroundBlock">components</span>两个文件夹，来存放项目需要用到的页面和组件。
安装好之后，在<span class="backgroundBlock">src</span>目录下新建一个<span class="backgroundBlock">router.js</span>文件，记得**配置好路径为空或者不存在时的映射**（一般是跳转到首页或登录页）。
```javascript router.js
import Vue from 'vue'
import VueRouter from 'vue-router'

// 引入（路由）组件
import Login from './views/login.vue'
import Home from './views/home.vue'

// 告诉 Vue 使用 VueRouter
Vue.use(VueRouter)

// 定义路由router
const router = new VueRouter({
    routes: [
        {
            path: '/',
            redirect: {
                name: "login"
            }
        },
        {
            path: '*',
            redirect: {
                name: "login"
            }
        },
        {
            path: '/login',
            name: 'login',
            component: Login,
            meta:{
                name:'登录'
            }
        },
        {
            path: '/home',
            name: 'home',
            component: Home,
            meta:{
                name:'首页'
            }
        },
    ]
});

// 将router暴露出去供其他文件使用
export default router;
```
在<span class="backgroundBlock">main.js</span>中引用<span class="backgroundBlock">router.js</span>
```javascript main.js
import Vue from 'vue'
import App from './App.vue'     
import router from './router'      // +

Vue.config.productionTip = false

new Vue({
    router,                       // +
    render: h => h(App),
}).$mount('#app')
```
<span class="backgroundBlock">App.vue</span>中，添加一个<span class="backgroundBlock">&lt;router-view /&gt;</span>标签作为路由的出口，我们可以通过进入不同的路由来显示不同的页面。
```html App.vue
<template>
    <div id="app">
        <router-view />
    </div>
</template>

<script>
    export default {
        name: 'App',
        components: {
        }
    }
</script>

<style>
    #app {
    }
</style>
```
此时项目目录结构如图所示：
<div style="text-align:center;"><img src="/cata1.png"></div>


4. **安装CSS预处理器（Scss）**
``` sh
npm install sass-loader node-sass --save-dev
```
安装完css预处理器之后，我们就能写入如下格式的css代码了：
```css
.login {
    height: 100vh;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 26vh;
    background: #f0f3f7;
    .content {
        width: 368px;
        .title {
            text-align: center;
            margin-bottom: 24px;
            font-size: 24px;
        }
        .input {
            margin-bottom: 24px;
        }
        .btn {
            width: 100%;
        }
    }
}
```

5. **登录页**
写页面过程中会用到 Ant Design 的组件，引用方法如下：
```js main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

import {
    Input,
    Button,
    Menu,
    Icon,
} from 'ant-design-vue';

// Vue.component和Vue.use这里作用都是注册组件
// Vue.component(Input.name, Input);
Vue.use(Input)
Vue.use(Button)
Vue.use(Menu)
Vue.use(Icon)

Vue.config.productionTip = false

new Vue({
    router,
    render: h => h(App),
}).$mount('#app')
```
完成好登录页面：
<div style="text-align:center;"><img src="/login.png"></div>
