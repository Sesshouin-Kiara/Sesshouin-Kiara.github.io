---
title: 使用vuedraggable进行拖拽操作
date: 2020-06-08 14:08:50
categories:
- Vue
tags:
- Vue
- vuedraggable
- 拖拽
thumbnail: https://s2.ax1x.com/2020/01/08/lgtuXd.jpg
---
公司新项目需要实现拖拽排序的功能，包括 列表拖拽、表格拖拽、卡片拖拽 等等。这些不同的拖拽需求其原理是一样的，只不过是样式的区别而已。

我使用了<span class="backgroundBlock">vuedraggable</span>这个插件，这里记录一下基本的使用方法。
<!-- more -->
```sh 安装
npm install vuedraggable
```
直接上代码：
```html html
<template>
    <content-view class="home-page">
        <div class="bg-white pt24 pr32 pl32 pb32 content">
            <!-- 添加拖拽功能时，只需要添加一个 draggable 标签就行了 -->
            <draggable
                v-model="listData"
                @start="drag=true"
                @end="drag=false"
                @change="change"
            >
                <!-- 遍历数据，渲染卡片 -->
                <div
                    class="card"
                    v-for="(item,index) in listData"
                    :key="index"
                >
                    <div class="left">
                        <div
                            class="avatar"
                            :style="`background-image:url(${item.avatar})`"
                        ></div>
                    </div>
                    <div class="right">
                        <div class="name">姓名：{{item.name}}</div>
                        <div class="age">年龄：{{item.age}}</div>
                        <div class="age">地址：{{item.address}}</div>
                    </div>
                </div>
            </draggable>
        </div>
    </content-view>
</template>
```
```js js
<script>
import draggable from "vuedraggable";    // 引入vuedraggable
export default {
    data() {
        return {
            listData: []
        };
    },
    components: {
        draggable                       // 注册vuedraggable组件
    },
    created() {
        this.getList();
    },

    methods: {
        getList() {
            this.listData = [];
            for (let i = 1; i < 6; i++) {
                this.listData.push({
                    id: i,
                    name: `王${i}`,
                    age: 20 + i,
                    address: `海军本部G${i}`,
                    avatar:
                        "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591597325670&di=504d93541846972ea08c453d3113ac9e&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201710%2F06%2F20171006232402_CsVeK.thumb.700_0.jpeg"
                });
            }
        },
        // 拖拽时的触发事件
        change({moved}) {
            console.log('当前拖动的卡片：',moved);
            console.log('整个卡片数组：',this.listData)
        }
    }
};
</script>
```
```css css（这一块没必要看，可以收起来。。。）
<style lang="scss">
.content {
    .card {
        width: 420px;
        height: 230px;
        margin-bottom: 32px;
        border-radius: 6px;
        display: flex;
        padding: 16px;
        background-color: rgba($color: #000000, $alpha: 0.05);
        .left {
            height: 100%;
            width: 40%;
            .avatar {
                height: 100%;
                background-repeat: no-repeat;
                background-size: cover;
                background-position: center;
                border-radius: 6px;
            }
        }
        .right {
            flex: 1;
            height: 100%;
            display: flex;
            flex-direction: column;
            font-size: 18px;
            justify-content: space-around;
            padding-left: 24px;
        }
    }
}
</style>
```
效果图：
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog20_img2.png"></div>

在被拖拽组件的外层套一个<span class="backgroundBlock">draggable</span>标签，并将渲染卡片的数据<span class="backgroundBlock">listData</span>绑定到<span class="backgroundBlock">draggable</span>标签的<span class="backgroundBlock">v-model</span>中，就可以使用拖拽功能了。

每次拖拽时，都会触发<span class="backgroundBlock">change</span>事件。一般业务逻辑的代码都会写在<span class="backgroundBlock">change</span>事件中，而这里面我们一般会用到两个数据：<span class="backgroundBlock">listData</span>和<span class="backgroundBlock">moved</span>。

<span class="importantBlock">listData是渲染的数据源，和draggable的v-model是双向绑定的。</span>拖拽发生后，listData会自动改变。

<span class="importantBlock">moved是change()默认参数的一个属性</span>，将其打印出来，发现这是一个由<span class="importantBlock">被拖拽的对象、拖拽之前的索引、拖拽之后的索引</span>所组成的对象。
<div style="text-align:center;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog20_img3.png"></div>