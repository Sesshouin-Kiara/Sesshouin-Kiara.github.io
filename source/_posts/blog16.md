---
title: 从零开始搭建后台管理系统（三）Ajax、Axios
date: 2020-03-18 10:52:58
categories:
- Ant Design
- [Vue,Axios]
tags:
- Vue
- Ant Design
- Axios
thumbnail: https://s2.ax1x.com/2020/01/03/lUPGBn.jpg
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
>9. 
>10. 
<!-- more -->

### **8.Ajax 以及 Axios 的封装**
后台接口使用豆瓣提供的开放接口：
获取top250电影：https://douban.uieee.com/v2/movie/top250
#### **8.1. Ajax**
新建一个电影模块的页面，直接上代码：
```html src/views/movie/top250.vue
<template>
    <div class="movie">
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
以上是原生ajax的用法，先创建一个xhr对象，然后调用open()和send()方法发送请求。在readyState=4的readystatechange事件回调中进行操作。
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
实际开发中，接口通常比较多，这样使用axios并不是很方便，我们需要对axios进行封装。