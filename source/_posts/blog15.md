---
title: 从零开始搭建后台管理系统（三）Vuex
date: 2020-03-17 10:10:02
categories:
- Ant Design
- [Vue,Vuex]
tags:
- Vue
- Ant Design
- Vuex
thumbnail: https://s2.ax1x.com/2020/01/08/lgtuXd.jpg
---
本项目基于 Vue全家桶 和 Ant Design 进行开发。
>目录
>1. 安装脚手架工具（vue-cli）、创建新项目
>2. 引入 Ant Design
>3. 引入 Vue Router
>4. 引入 node-sass/sass-loader
>5. 登录页
>6. 完成页面的基本布局（ 根据路由渲染菜单组件、面包屑组件等 ）
>7. **使用 Vuex 完成菜单的显示隐藏**
>8. Ajax 以及 Axios 的封装
>9. 全局方法 Utils、混合 Mixins
>10. 
<!-- more -->


### **7.使用Vuex完成菜单的显示隐藏**
把控制菜单显示隐藏的按钮写在菜单组件中毫无难度，但是我们希望把这个按钮移到头部组件中，对于跨组件的通信，需要使用 [Vuex](https://vuex.vuejs.org/zh/)。

趁此机会系统地梳理一下 Vuex 的用法。

```sh 安装
$ npm install vuex --save
```
#### **7.1. state**
在 src 目录下新建 store/index.js 文件：
```js store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        collapsed: false,
        logoShow: true,
    },
})

export default store
```
在 main.js 中引入并使用 store：
```js main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'    // +

import {
    Input,
    Button,
    Menu,
    Icon,
    Breadcrumb,
} from 'ant-design-vue';

Vue.use(Input)
Vue.use(Button)
Vue.use(Menu)
Vue.use(Icon)
Vue.use(Breadcrumb)

Vue.config.productionTip = false

new Vue({
    router, 
    store,                  // +
    render: h => h(App),
}).$mount('#app')
```
在其他页面和组件中能通过 **this.$store.state** 或者 **mapState** 拿到 store.state 中定义的值。
#### **7.2. getters**
在 store/index.js 中添加 getter：
```js store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        collapsed: false,
        logoShow: true,
    },
    getters: {
        // es6语法,箭头函数更简洁
        collapsedText: state => 'hahaha' + state.collapsed,
        logoShowText: state => 'hahaha' + state.logoShow,
    }
})

export default store
```
Getter相当于vue中的computed计算属性，在其他页面和组件中可以通过 **this.$store.getters** 或者 **mapGetters** 拿到值。
```js xxx.vue
import { mapState,mapGetters } from "vuex";
export default {
    computed: {
        ...mapState(["collapsed","logoShow"]),// 将 `this.collapsed` 映射为 `this.$store.state.collapsed`
        ...mapGetters(["collapsedText","collapsedText"]),// 将 `this.collapsedText` 映射为 `this.$store.getters.collapsedText
    },
    created() { 
        console.log(this.$store.state.collapsed) // 通过$store取值
        console.log(this.collapsed,this.logoShow,this.collapsedText,this.collapsedText) // 通过 mapState,mapGetters
    },
}
```
#### **7.3. mutations**
在 store/index.js 中继续添加 mutations：
```js store/index.js
const store = new Vuex.Store({
    //......
    mutations: {
        changeShowState(state) {
            state.collapsed = !state.collapsed
            state.logoShow = !state.logoShow
        }
    },
    //......
})
```
在其他页面和组件中可以使用 **this.$store.commit('changeShowState')** 或者 **mapMutations** 来调用mutation去修改state中的值。

修改 aside 和 header 组件的相关代码：
```html aside.vue
<template>
    <div class="myAside">
        <div class="logo" v-if="logoShow">阿里巴巴后台管理系统</div>
        <!-- 菜单组件 -->
        <a-menu
            :defaultSelectedKeys="selectedKeys"
            :inlineCollapsed="collapsed"
            :openKeys.sync="openKeys"
            mode="inline"
            theme="dark"
        >
            <template v-for="item in routeData">
                <!-- 有children的路由 -->
                <!-- 省略 -->
                <!-- 无children的路由 -->                
                <!-- 省略 -->
            </template>
        </a-menu>
    </div>
</template>

<script>
    import { mapState } from "vuex";
    export default {
        data() {
            return {
                routeData: [],
                selectedKeys: [],
                openKeys: [],
            };
        },
        
        computed: {
            ...mapState(["collapsed","logoShow"]),
        },

        created() {
            // 省略
        },

        methods: {
            // 省略
        }
    }
</script>
<style lang="scss">
</style>
```
```html header.vue
<template>
    <div class="myHeader">
        <a-button
            @click="toggleCollapsed"
            type="primary"
        >
            <a-icon :type="collapsed ? 'menu-unfold' : 'menu-fold'" />
        </a-button>
        <div class="myHeader-right">
            <div>马云</div>
            <div style="margin:0 16px;color:blue">退出登录</div>
        </div>
    </div>
</template>

<script>
    import { mapState, mapGetters, mapMutations } from "vuex";
    export default {
        data() {
            return {
            };
        },

        computed: {
            ...mapState(["collapsed"]),
        },

        methods: {
            ...mapMutations(['changeShowState']),// 将 `this.changeShowState()` 映射为 `this.$store.commit('changeShowState')`
            toggleCollapsed() {
                // this.$store.commit('changeShowState')
                this.changeShowState()
            },
        }
    }

</script>
<style lang="scss">
    .myHeader {
        height: 66px;
        background-color: #fff;
        font-size: 14px;
        box-shadow: 5px 0px 10px #ccc;
        display: flex;
        padding: 0 24px;
        justify-content: space-between;
        align-items: center;
        &-right {
            display: flex;
            justify-content: space-between;
        }
    }
</style>
```
修改之后，可以点击头部组件的按钮来控制菜单的显示隐藏了：
<div style="text-align:center;"><img src="/print3.png"></div>
<div style="text-align:center;"><img src="/print4.png"></div>
  

#### **7.4. actions**
通过 Action 提交 mutation，再通过 mutation 去改变 state，乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？
实际上并非如此，mutation 必须是同步函数，任何在回调函数中进行的状态的改变都是不可追踪的。在 mutation 中混合异步调用会导致程序很难调试，为了处理异步操作，我们需要 Action。
```js store/index.js
const store = new Vuex.Store({
    state: {
        collapsed: false,
        logoShow: true,
    },
    getters: {
        // ......
    },
    mutations: {
        changeShowState(state) {
            state.collapsed = !state.collapsed
            state.logoShow = !state.logoShow
        }
    },
    actions: {
        changeShowStateAsync({ commit }) { 
            // 在action中加上1s的延迟表示异步，
            // 实际开发中是在各种api的回调函数中commit mutation。
            setTimeout(() => {
                commit('changeShowState')
            }, 1000);
        }
    },
})
```
Action 通过 **this.$store.dispatch** 方法触发。我们将header组件中按钮的点击事件函数修改为使用 Action：
```js header.vue
toggleCollapsed() {
    // this.$store.commit('changeShowState')   // mutation
    // this.changeShowState()                  // mutation
    this.$store.dispatch('changeShowStateAsync')    // action
},
```
现在点击按钮，效果变成了点击事件触发后1s菜单才隐藏/显示。
同样，Actions 也可以通过 **mapActions** 来简化写法。
```js header.vue
import { mapState, mapGetters, mapMutations,mapActions } from "vuex";
export default {
    data() {
        return {
        };
    },

    computed: {
        // 将 `this.collapsed` 映射为 `this.$store.state.collapsed`
        ...mapState(["collapsed"]),
        // 将 `this.collapsedText` 映射为 `this.$store.getters.collapsedText
        ...mapGetters(["collapsedText"]),
    },

    created() { },

    methods: {
        // 将 `this.changeShowState()` 映射为 `this.$store.commit('changeShowState')`
        ...mapMutations(['changeShowState']),
        // 将 `this.changeShowStateAsync()` 映射为 `this.$store.dispatch('changeShowStateAsync')`
        ...mapActions(['changeShowStateAsync']),
        toggleCollapsed() {
            // this.$store.commit('changeShowState')           // mutation
            // this.changeShowState()                          // mutation 的映射
            // this.$store.dispatch('changeShowStateAsync')    // action
            this.changeShowStateAsync()                        // action 的映射
        },
    }
}
```
当然，仅就本节的显示隐藏菜单功能没必要使用Action。
> 其实我在 mutations 中使用异步操作一样可以正确执行，也就是说异步Mutation不会对数据造成丢失或其他影响。
>
> vuex原文解释：『在 mutation 中混合异步调用会导致你的程序很难调试。例如，当你能调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？这就是为什么我们要区分这两个概念。在 Vuex 中，mutation 都是同步事务。』
>
> 我们使用Devtools去看state的变化，当我们去查看多次Mutation状态时，发现同步的显示Ok，异步Mutation的 state 数据显示和我们预期结果不一致，所以会造成状态改变的不可追踪，所以官方说我们Mutation是同步的。
> 
mutation 的提交是对state做确定的 、立即生效的修改。