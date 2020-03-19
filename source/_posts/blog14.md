---
title: 从零开始搭建后台管理系统（二）Vue Router
date: 2020-03-14 14:32:26
categories:
- Ant Design
- [Vue,Vue Router]
tags:
- Vue
- Ant Design
- Vue Router
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
本项目基于 Vue全家桶 和 Ant Design 进行开发。
>目录
>1. 安装脚手架工具（vue-cli）、创建新项目
>2. 引入 Ant Design
>3. 引入 Vue Router
>4. 引入 node-sass/sass-loader
>5. 登录页
>6. **完成页面的基本布局（ 根据路由渲染菜单组件、面包屑组件等 ）**
>7. 使用 Vuex 完成菜单的显示隐藏
>8. Ajax 以及 Axios 的封装
>9. 全局方法 Utils、混合 Mixins
>10. 
<!-- more -->

### **6.完成页面的基本布局**
> App.vue 中有一个 &lt;router-view /&gt; 标签作为路由的出口，这个出口可以用来匹配 Login 页 等不需要菜单、头部、面包屑、底部等公用组件的页面。但是对于菜单里面的功能页，每个页面都需要这些公用组件，如果用这个路由出口来匹配这些功能页的话，每个功能页都需要写入这些公用组件，显得十分臃肿。因此我们需要创建一个 main.vue 文件，来完成功能页面的布局（包括菜单、头部、底部组件以及显示页面对应功能内容的区域），并且在 main.vue 中再提供一个 &lt;router-view /&gt; 标签，作为功能页面路由的出口，用来显示所有需要用到菜单等组件的功能页面。
* 在<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">views</span>目录下创建<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">main.vue</span>文件。
* 在<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">components</span>目录下创建 <span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">aside（侧边菜单）</span>、<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">breadcrumb（面包屑）</span>、<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">header（头部）</span> 3个组件文件；
* 在<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">views</span>目录下创建 account（账号）、department（部门）、setting（设置） 3个功能页面的文件夹。
* 把新页面添加进<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">router.js</span>
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
                flex: 1;  //头部和底部组件高度确定，整体min-height又等于100vh（aside高度），所以内容区填满剩余的高度
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

不过出现了一个小问题，我们使用<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">main.vue</span>来布局，使用<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">Main</span>的子路由来显示功能页的内容，这样导致首页、设置等只需要一级路由的模块有了两层路由，菜单中也出现了两级菜单，这不是我们期望的。
**解决方案：**
我们在路由元信息<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">meta</span>中添加一个<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">unfold</span>字段来表示该项路由对应菜单的是否能展开，为 true 时表示该项菜单可以展开。
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
<span style="background-color:rgb(245,245,245);padding:3px 6px;margin-right:6px;">aside</span>组件中，菜单能否展开的判断条件从 是否有子路由<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">children</span>改成 路由元信息中的<span style="background-color:rgb(245,245,245);padding:3px 6px;margin:0 6px;">unfold</span>字段是否存在且为true。
```html aside.vue
<!-- 有children的路由 -->
<a-sub-menu
    :key="item.name"
    v-if="item.meta.unfold"
>
```
修改后效果就比较符合原来的预期了：
<div style="text-align:center;"><img src="/layout3.png"></div>
