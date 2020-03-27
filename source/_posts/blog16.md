---
title: 从零开始搭建后台管理系统（四）Ajax、Axios
date: 2020-03-18 10:52:58
categories:
- Ant Design
- [Vue,Axios]
tags:
- Vue
- Ant Design
- Axios
thumbnail: https://s2.ax1x.com/2019/12/31/l3E5gs.jpg
---
本项目基于 Vue全家桶 和 Ant Design 进行开发。
>目录
>1. 安装脚手架工具（vue-cli）、创建新项目
>2. 引入 Ant Design
>3. 引入 Vue Router
>4. 引入 node-sass/sass-loader
>5. 登录页
>6. 完成页面的基本布局（ 根据路由渲染菜单组件、面包屑组件等 ）
>7. 使用 Vuex 完成菜单的显示隐藏
>8. **Ajax 以及 Axios 的封装**
>9. 全局方法 Utils、混合 Mixins
>10. 后续功能开发
<!-- more -->

### **8.Ajax 以及 Axios 的封装**
后台接口使用豆瓣提供的开放接口：
获取top250电影：https://douban.uieee.com/v2/movie/top250
#### **8.1. Ajax**
新建一个电影模块的页面，直接上代码：
```html src/views/movie/top250.vue
<template>
    <div class="movie">
        <!-- rowKey不能少 -->
        <a-table
            :columns="columns"
            :dataSource="data"
            :rowKey="record => record.id"
            bordered
        >
            <div
                slot="images"
                slot-scope="text"
            >
                <div style="height:120px;">
                    <img
                        :src="text.small"
                        alt="img"
                        style="height:100%;"
                    />
                </div>
            </div>
            <div
                slot="rating"
                slot-scope="text"
            >{{text.average}}</div>
            <div
                slot="durations"
                slot-scope="text"
            >{{text[0]}}</div>
            <div
                slot="genres"
                slot-scope="text"
            >
                <a-tag
                    :key="index"
                    color="cyan"
                    v-for="(tag,index) in text"
                >{{tag}}</a-tag>
            </div>
            <div
                slot="action"
                slot-scope="record"
            >
                <a
                    @click="delMovie(record.id)"
                    slot="action"
                >收藏</a>
                <span style="color:#eee;padding:0 4px;">|</span>
                <a
                    @click="delMovie(record.id)"
                    slot="action"
                >喜欢</a>
                <span style="color:#eee;padding:0 4px;">|</span>
                <a
                    @click="delMovie(record.id)"
                    slot="action"
                >删除</a>
            </div>
        </a-table>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                data: [],
                columns: [
                    {
                        title: "编号",
                        customRender: (text, record, index) =>
                            `${index + 1}`
                    },
                    { title: '名称', dataIndex: 'title' },
                    { title: '海报', dataIndex: 'images', scopedSlots: { customRender: 'images' } },
                    { title: '评分', dataIndex: 'rating', scopedSlots: { customRender: 'rating' } },
                    { title: '时长', dataIndex: 'durations', scopedSlots: { customRender: 'durations' } },
                    { title: '标签', dataIndex: 'genres', scopedSlots: { customRender: 'genres' } },
                    { title: '年份', dataIndex: 'year' },
                    {
                        title: '操作',
                        fixed: 'right',
                        width: 150,
                        scopedSlots: { customRender: 'action' },
                    }
                ]
            };
        },

        created() {
            this.getList()
        },

        methods: {
            getList() {
                //原生ajax
                let that = this
                var xhr = new XMLHttpRequest()
                xhr.onreadystatechange = function () { //异步回调，每当readyState改变的时候都会触发readystatechange事件
                    if (xhr.readyState == 4) {
                        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
                            that.data = JSON.parse(xhr.responseText).subjects
                        } else {
                            console.log("Request was unsuccessful: " + xhr.status);
                        }
                    }
                };
                xhr.open("get", "https://douban.uieee.com/v2/movie/top250?count=30", false); // 准备
                xhr.send(null)                                                      // 发送
            },
            delMovie(id) { }
        }
    }
</script>

<style lang="scss">
    .movie {
        // 基本布局
        padding: 24px;
        background-color: white;
        min-height: 100%;
    }
</style>
```
以上是原生ajax的用法，先创建一个xhr对象，然后调用open()和send()方法发送请求。在 readyState=4 的 readystatechange 事件回调中进行操作。
效果如下，成功拿到了豆瓣的数据：
<div style="text-align:center;"><img src="/print1.png"></div>

> 使用豆瓣API的过程中发现了一个**问题**，管理系统请求到的图片资源在 network 中全部是403禁止访问，导致在网页中不能正常显示，但是把图片url放到浏览器中可以正常打开图片。
> **原因**是豆瓣有防盗链，后台会根据请求头header里的 Referrer 属性来判断请求是不是来自一个白名单内的网站，如果不是就返回 403 forbidden。
> **解决方法**如下，我们把 Referrer 信息去掉就好了。在 index.html 文件中添加一个 **&lt;meta name="referrer" content="never"&gt;** 标签。

#### **8.2. Axios**
接下来看看 [Axios（查看文档）](http://www.axios-js.com/zh-cn/docs/) 的用法。
安装：
```sh
$ npm install axios
```
在需要用到axios的页面引入
```js top250.vue
import axios from "axios";
//......
getList() { 
    //axios
    axios.get('https://douban.uieee.com/v2/movie/top250?count=30').then((response) => {
        console.log(response.data.subjects);
        this.data = response.data.subjects
    }).catch(function (error) {
        console.log(error);
    });
},
```
或者：
```js top250.vue
import axios from "axios";
//......
getList() { 
    // 上面的请求也可以这样写
    axios.get('https://douban.uieee.com/v2/movie/top250', {
        params: {
            count: 30
        }
    }).then((response) => {
        console.log(response.data.subjects);
        this.data = response.data.subjects
    }).catch(function (error) {
        console.log(error);
    });
},
```
我们不希望每个页面都去引入一次axios，更重要的是，实际开发中，接口通常会很多，需要**分模块、系统**地管理api，所以我们需要对axios进行封装。

#### **8.3. 对 Axios 进行封装**
在src目录下新建axios.js文件，用来写axios的配置项和拦截器等。
在src目录下新建一个api文件夹。api/modules文件夹用来存放所有api模块；api/index.js文件用来统一封装ajax方法。
目录如下：
<div style="text-align:center;"><img src="/print2.png"></div>

```js src/axios.js
import Vue from 'vue';
import axios from "axios";
import router from "./router";
// import UserInfoTool from './user-info-tool';
import {
    message as Message
} from 'ant-design-vue';

// 实际开发中需要根据不同的环境选择不同的api_baseUrl
// const _domain = process.env.VUE_APP_API_URL;
const _domain = 'https://douban.uieee.com';


let config = {
    baseURL: _domain,
    timeout: 60 * 1000, // Timeout
    withCredentials: true, // Check cross-site Access-Control
    validateStatus: function (status) {
        return status >= 200 && status < 500; // default
    },
};

const _axios = axios.create(config);

// 添加请求拦截器
_axios.interceptors.request.use(
    function (config) {
        // 在发送请求之前做些什么

        // 如果登录存了token的话，在这里把token放入请求头
        // let _token = UserInfoTool.getToken();
        // if (_token) {
        //     config.headers.authorization = _token;
        // }

        return config;
    },
    function (error) {
        // 对请求错误做些什么
        return Promise.reject(error);
    }
);

// 添加响应拦截器
_axios.interceptors.response.use(
    function (response) {
        // 对响应数据做点什么        
        if (/blob/i.test(response.config.responseType)) {
            // 如果是blob数据的话，返回全部response
            return response;
        } else if (response.status === 200) {
            // 如果响应头中携带authorization等信息，重置token
            // if (response.headers.authorization) {
            //     UserInfoTool.setToken(response.headers.authorization);
            // }
            return response.data;
        } else if (response.status === 401) {
            Message.error('登录失效，请重新登录');
            // 清除账号token信息，返回登录页面
            // UserInfoTool.clear();
            router.push({
                path: '/login'
            })
            return Promise.reject(new Error(response.msg));
        } else {
            Message.error(response.msg);
            return Promise.reject(new Error(response.msg));
        }
    },
    function (error) {
        // 对响应错误做点什么
        return Promise.reject(error);
    }
);

Vue.prototype.$domain = _domain;
Vue.prototype.$axios = _axios;
Vue.prototype.$baseUrl = config.baseURL;
export default _axios;
export const domain = _domain;
```
```js src/api/modules/movie.api.js
export default {
    getTop250: {
        method: 'GET',
        url: '/v2/movie/top250',
        name: '获取top250电影列表'
    },
}
```
```js src/api/index.js
/**
 * api 统一封装方法
 * @description api集合为元数据，可直接在页面使用 this.$api[module][api]某某方法
 * @example api.login.logout(queryOrBodyData, enableFilterEmpty)
 *
 * @param {Object} queryOrBodyData? 查询参数 或者 荷载数据
 * @param {Boolean} enableFilterEmpty? 是否开启过滤空字段，默认 false
 *
 * @return {Object<{ data }>}
 */

import {
    message as Message
} from 'ant-design-vue';
import axios, {
    domain
} from '@/axios';

// 引入api模块
import movieApi from './modules/movie.api';

// 合并所有模块api并导出到外部， 可直接在页面使用 this.$api.模块名.接口名
const APIS = {
    movie: movieApi,
};

/**
 * 请求前 中间件
 * @param apiMeta API元数据
 * @param data 请求数据
 */
function foreRequestMiddleWare(apiMeta, data) {
    const {
        url,
        method,
    } = apiMeta;

    // 处理 /a/b/c/:id 接口
    if (method.toUpperCase() === 'GET' || method.toUpperCase() === 'DELETE') {
        apiMeta.targetUrl = url.replace(/\B:(\w+)/g, (...args) => {
            apiMeta.resource = true;
            return data[args[1]];
        })
    } else {
        apiMeta.targetUrl = url;
    }
};

/**
 * 请求失败后 中间件
 * @param {ApiMeta} apiMeta API元数据
 * @param {Error} error 请求错误对象
 * @param {Function} next 下一步
 */
function fallbackMiddleWare(apiMeta, error, next) {
    const {
        name,
        method,
        url,
        errorHandler
    } = apiMeta;
    console.error(`[${method}][${domain}${url}]: ${name} 失败，${error.message}`);
    if (errorHandler) {
        Message.error(`${name} 失败，${error.message}`);
    }
    next(error);
}

// 将单个模块的 meta 转换为 请求
function Proxyable(target) {
    const target_ = {};
    for (const key in target) {
        if (target.hasOwnProperty(key)) {
            target_[key] = ProxyApi(target, key);
        }
    }
    return target_;
}

// 将 ApiMeta 映射为 http 请求
function ProxyApi(target, key) {
    if (!target[key]) throw new ReferenceError('API ' + key + ' not exist');
    const {
        method,
    } = target[key];
    if (method.toUpperCase() === 'GET') {
        return (query = {
        }, opt) => new Promise((resolve, reject) => {
            foreRequestMiddleWare(target[key], query);
            let promise = null;
            if (target[key].resource) {
                promise = axios.get(target[key].targetUrl, {}, opt)
            } else {
                promise = axios.get(target[key].targetUrl, Object.assign({}, {
                    params: query
                }, opt))
            }
            promise
                .then(data => resolve(data))
                .catch(err => {
                    fallbackMiddleWare(target[key], err, reject)
                });
        })
    } else if (method.toUpperCase() === 'PATCH') {
        return (body = {}, opt) => new Promise((resolve, reject) => {
            foreRequestMiddleWare(target[key], body);
            axios.patch(target[key].targetUrl, body, opt)
                .then(data => resolve(data))
                .catch(err => {
                    fallbackMiddleWare(target[key], err, reject)
                });
        })
    } else if (method.toUpperCase() === 'POST') {
        return (body = {}, opt) => new Promise((resolve, reject) => {
            foreRequestMiddleWare(target[key], body);
            axios.post(target[key].targetUrl, body, opt)
                .then(data => resolve(data))
                .catch(err => {
                    fallbackMiddleWare(target[key], err, reject)
                });
        })
    } else if (method.toUpperCase() === 'DELETE') {
        return (query = {}, opt) => new Promise((resolve, reject) => {
            foreRequestMiddleWare(target[key], query);
            let promise = null;
            if (target[key].resource) {
                promise = axios.delete(target[key].targetUrl, {}, opt)
            } else {
                promise = axios.delete(target[key].targetUrl, Object.assign({}, {
                    params: query
                }, opt))
            }
            promise
                .then(data => resolve(data))
                .catch(err => {
                    fallbackMiddleWare(target[key], err, reject)
                });
        })
    } else if (method.toUpperCase() === 'PUT') {
        return (body = {}, opt) => new Promise((resolve, reject) => {
            foreRequestMiddleWare(target[key], body);
            axios.put(target[key].targetUrl, body, opt)
                .then(data => resolve(data))
                .catch(err => {
                    fallbackMiddleWare(target[key], err, reject)
                });
        })
    } else {
        throw new Error('【API】API源数据信息错误，不支持的方法：' + method);
    }
}

const API_ = {};

Object.keys(APIS).forEach(apiName => {
    API_[apiName] = Proxyable(APIS[apiName]);
});

export default API_;
```
> 当系统有很多功能模块、每个模块都有许多接口时，我们可以按不同的功能模块把 api 写在不同的 src/api/modules/xxx(模块名).api.js 文件中，每个 xxx.api.js 文件只写本模块相关的 api。

记得在main.js中引入axios相关文件，这样就不用每个文件引入axios了。
（ 引用和依赖关系：main.js <== api/index.js <== axios.js ）
```js main.js
import api from './api';

Vue.prototype.$api = api;
```
现在，我们可以直接通过 this.$api.[模块名].[接口名] 的形式在任意页面调用接口了。
views/movie/top250.vue 中的 getList 方法可以改成这样：
```js top250.vue
getList() {
    this.$api.movie.getTop250({
        count: 30
    }).then(data => {
        this.data = data.subjects
    }).catch(err => {
        //
    });
},
```