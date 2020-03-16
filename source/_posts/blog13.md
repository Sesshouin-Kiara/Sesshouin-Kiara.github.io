---
title: 从零开始搭建后台管理系统
date: 2020-03-12 16:25:19
categories:
- [技术,Ant Design]
- [技术,Vue]
tags:
- Vue
- Ant Design
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
本项目基于 Vue全家桶 和 Ant Design 进行开发。
>目录
>1. 安装脚手架工具（vue-cli）、创建新项目
>2. 引入Ant Design
>3. 引入Vue Router
>4. 引入node-sass/sass-loader
>5. 登录页
>6. 完成页面的基本布局1（ 根据路由渲染菜单 ）
>7. 完成页面的基本布局2（ header、footer、面包屑等组件 ）
>8. 
>9. 
>10. 
>11. 引入Axios、Vuex等插件,权限,xxx,xxx,xxx,xxx,xxx,xxx
<!-- more -->

>tips：
>1. npm下载速度慢可使用 cnpm 或 yarn。
>2. 安装各种插件过程中项目可能会报缺少一些依赖之类的错误，安装这些依赖包后仍然报错，大概率是node包版本的问题，建议百度。
>3. eslint有点烦，不想配置的话建议新建项目的时候 eslint 选择 no

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
安装路由之前，我们可以在src目录下新建views和components两个文件夹，来存放项目需要用到的页面和组件。
安装好之后，在src目录下新建一个router.js文件，记得**配置好路径为空或者不存在时的映射**（跳转到登录页面）。
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
在main.js中引用router.js
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
App.vue中，添加一个&lt;router-view /&gt;标签作为路由的出口，我们可以通过进入不同的路由来显示不同的页面。
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

6. **完成页面的基本布局**
> App.vue 中有一个&lt;router-view /&gt;标签作为路由的出口，这个出口可以用来匹配 Login页 等不需要菜单、头部、面包屑、底部组件的页面。但是对于菜单里面的功能页，每个页面都需要菜单、头部、面包屑、底部等公用组件，如果用这个路由出口来匹配这些功能页的话，这些功能页每个都需要写入这些公用组件，显得十分臃肿。我们需要创建一个main.vue文件，来完成功能页面的布局（包括左边的菜单、头部、底部组件以及显示页面对应功能内容的区域），并且再提供一个&lt;router-view /&gt;用来匹配所有这些需要用到菜单等组件的功能页面。
* 在views 目录下创建 main.vue 文件。
* 在 components 目录下创建 aside（侧边菜单）、breadcrumb（面包屑）、header（头部） 3个组件文件；
* 在 views 目录下创建 account（账号）、department（部门）、setting（设置） 3个功能页面的文件夹。
* 把新页面添加进router.js
<div style="text-align:center;"><img src="/cata2.png"></div>

```js router.js
import Vue from 'vue'
import VueRouter from 'vue-router'

// 引入（路由）组件
// 基本布局Main页面
import Main from './views/main'

import Login from './views/login.vue'
import Home from './views/home'
//账号管理
import Account from './views/account'
//部门管理
import FrontEnd from './views/department/front-end'
import BackEnd from './views/department/back-end'
//设置
import Setting from './views/setting'

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
            meta: {
                name: '登录'
            }
        },
        {
            path: '/home',
            name: 'home',
            component: Main,
            meta: {
                name: '首页'
            },
            redirect: {
                name: 'homeIndex'
            },
            children: [
                {
                    path: 'index',
                    name: 'homeIndex',
                    meta: {
                        name: '首页',
                        auth: true,
                    },
                    component: Home
                }
            ]
        },
        {
            path: '/account',
            name: 'account',
            component: Main,
            meta: {
                name: '账号管理'
            },
            redirect: {
                name: 'accountIndex'
            },
            children: [
                {
                    path: 'index',
                    name: 'accountIndex',
                    meta: {
                        name: '账号列表',
                        auth: true,
                    },
                    component: Account
                }
            ]
        },
        {
            path: '/department',
            name: 'department',
            component: Main,
            meta: {
                name: '部门管理'
            },
            redirect: {
                name: "front-end"
            },
            children: [
                {
                    path: 'front-end',
                    name: 'front-end',
                    component: FrontEnd,
                    meta: {
                        name: '前端开发部'
                    }
                }, {
                    path: 'back-end',
                    name: 'back-end',
                    component: BackEnd,
                    meta: {
                        name: '后端开发部'
                    }
                }
            ]
        },
        {
            path: '/setting',
            name: 'setting',
            component: Main,
            meta: {
                name: '设置'
            },
            redirect: {
                name: 'settingIndex'
            },
            children: [
                {
                    path: 'index',
                    name: 'settingIndex',
                    meta: {
                        name: '设置',
                        auth: true,
                    },
                    component: Setting
                }
            ]
        },
    ]
});

// 将router暴露出去供其他文件使用
export default router;
```
```html aside.vue
<template>
    <div class="myAside">
        <div class="logo">阿里巴巴后台管理系统</div>
        <!-- 菜单组件 -->
        <a-menu
            :defaultSelectedKeys="selectedKeys"
            mode="inline"
            theme="dark"
            :openKeys.sync="openKeys"
        >
            <template v-for="item in routeData">
                <!-- 有children的路由 -->
                <a-sub-menu
                    :key="item.name"
                    v-if="item.children"
                >
                    <span slot="title">
                        <span>{{item.meta.name}}</span>
                    </span>
                    <template v-for="childrenItem in item.children">
                        <a-menu-item
                            :key="childrenItem.name"
                            @click="gotoRoute(childrenItem.name)"
                        >{{childrenItem.meta.name}}</a-menu-item>
                    </template>
                </a-sub-menu>
                <!-- 无children的路由 -->
                <a-menu-item
                    :key="item.name"
                    @click="gotoRoute(item.name)"
                    v-else
                >
                    <span>{{item.meta.name}}</span>
                </a-menu-item>
            </template>
        </a-menu>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                routeData: [],
                selectedKeys: [],
                openKeys: [],
            };
        },


        created() {
            this.getMenuData()
        },

        methods: {
            // 根据路由渲染菜单
            getMenuData() {
                //菜单项
                this.routeData = []
                let deepCloneRouteData = JSON.parse(JSON.stringify(this.$router.options.routes))
                // console.log(this.$router.history.current.name)
                deepCloneRouteData.forEach((val, index) => {
                    if ((val.path !== '/') && (val.path !== '*') && (val.path !== '/login')) {
                        this.routeData.push(val)
                    }
                })
                //默认选中项
                this.selectedKeys = [this.$router.history.current.name]
                //展开的子路由选中项
                this.openKeys = []
                this.openKeys = ['ios']
            },
            gotoRoute(name) {
                this.$router.push({
                    name: name
                })
            }
        }
    }
</script>
<style lang="scss">
    .myAside {
        width: 256px;
        min-height: 100vh; //使菜单高度填满屏幕
        background-color: #001529;
        .menuBtn {
            position: fixed;
            top: 10px;
            left: 260px;
            z-index: 999;
        }
        .logo {
            width: 256px;
            height: 60px;
            background-color: #001529;
            font-size: 16px;
            font-weight: 500;
            line-height: 60px;
            text-align: center;
            color: white;
        }
    }
</style>
```
```html breadcrumb.vue
<template>
    <div class="myBreadCurmb">
        <a-breadcrumb>
            <a-breadcrumb-item
                :key="index"
                v-for="(item,index) in path"
            >{{item}}</a-breadcrumb-item>
        </a-breadcrumb>
        <div class="title">{{title}}</div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                title: '',
                path: []
            };
        },

        components: {},

        computed: {},

        created() {
            this.setTitleAndPath()
        },
        watch: {
            $route(to, from) {
                this.setTitleAndPath()
            }
        },

        methods: {
            setTitleAndPath() {
                this.title = this.$router.history.current.meta.name
                this.path = []
                this.$router.history.current.matched.forEach((val, index) => {
                    this.path.push(val.meta.name)
                });
            }
        }
    }
</script>

<style lang="scss">
    .myBreadCurmb {
        padding: 16px 32px;
        margin-top: 3px;
        background-color: white;
        .title {
            font-size: 26px;
            font-weight: 600;
            color: black;
            margin-top: 16px;
        }
    }
</style>
```
```html main.vue
<template>
    <div class="myMain">
        <Aside></Aside>
        <div class="right">
            <Header></Header>
            <Breadcrumb></Breadcrumb>
            <div class="content">
                <router-view></router-view>
            </div>
            <div class="footer">Copyright © 2020 alibaba</div>
        </div>
    </div>
</template>
 
 <script>
    import Aside from '../components/aside'
    import Header from '../components/header'
    import Breadcrumb from '../components/breadcrumb'
    export default {
        data() {
            return {
            };
        },

        components: {
            Aside,
            Header,
            Breadcrumb
        },

        computed: {},

        created() { },

        methods: {}
    }
 </script>
 
 <style lang="scss">
    .myMain {
        display: flex;
        flex-direction: row;
        .right {
            flex: 1;
            display: flex;
            flex-direction: column;
            .content {
                padding: 24px 24px 0 24px;
                flex: 1;  //头部和底部组件高度确定，内容区填满剩余的高度
            }
            .footer {
                height: 76px;
                line-height: 76px;
                text-align: center;
                font-size: 12px;
                color: #aaa;
            }
        }
    }
</style>
```
基本的页面布局和路由已经搭建好了，我们看看效果：
<div style="text-align:center;"><img src="/layout1.png"></div>
<div style="text-align:center;"><img src="/layout2.png"></div>

不过出现了一个小问题，我们使用 main.vue 来布局，使用 Main 的子路由来显示功能页的内容，这样导致首页、设置等只需要一级路由的模块有了两层路由，菜单中也出现了两级菜单，这不是我们期望的。
**解决方案：**
我们在路由元信息meta中添加一个 unfold 字段来表示该项路由对应菜单的是否能展开，为 true 时表示该项菜单可以展开。
```js router.js
{
    path: '/department',
    name: 'department',
    component: Main,
    meta: {
        name: '部门管理',
        unfold: true   // +
    },
    redirect: {
        name: "front-end"
    },
    children: [
        //...
    ]
},
```
aside组件中，菜单是否能展开的判断条件从 是否有子路由children 改成 路由元信息中的unfold字段是否存在且为true
```html aside.vue
<!-- 有children的路由 -->
<a-sub-menu
    :key="item.name"
    v-if="item.meta.unfold"
>
```
修改后效果就比较符合原来的预期了：
<div style="text-align:center;"><img src="/layout3.png"></div>
