---
title: H5分享页面-展开全文、收起全文操作
date: 2020-05-9 17:32:56
categories:
- H5
tags:
- H5
- 移动端
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
在浏览器中打开从移动端应用分享出来的链接时，页面一般会有一些提示下载APP的浮块和展开全文、收起全文的操作。
<!-- more -->
例如初始状态的页面是无法上下滑动查看剩余内容的：
<div style="text-align:center;"><img style="width:300px;" src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog22_img1.png"></div>
点击展开全文后，页面就可以滑动查看下文了：
<div style="display:flex;"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog22_img2.png"><img src="https://wh-1301033226.cos.ap-nanjing.myqcloud.com/Hexo_img/blog_content/blog22_img3.png"></div>

点击收起全文，页面恢复到不可滑动状态。
  

**实现思路**如下：
>当处于收起全文状态时（className为showSome）: 
>1. 页面高度为100vh，多余内容隐藏。
>2. 显示展开全文按钮。

>页面处于展示全文状态时（className为showAll）:
>1. 页面高度为auto（内容多高页面就多高）。
>2. 显示收起全文按钮。

几个注意点：
>1. 收起全文状态时，整个页面高度为<span class="importantBlock">100vh（100vh代表屏幕可见区域的100%）</span>，由于下载浮块的关系，内容区可能只是整个页面的一部分，需要使用<span class="importantBlock">calc() 函数</span>计算一下高度：例如本例中收起全文状态时，.showSome的height为: calc(100vh-220px);

>2. 收起全文状态时，展开全文按钮及其周围部分遮挡了新闻详情的内容，并且有一个透明且渐变的效果。这里使用了<span class="importantBlock">background: linear-gradient</span>这个属性，该属性规定了<span class="importantBlock">渐变的方向和不同节点的状态</span>。
>```css
background: linear-gradient(
      to bottom,
      rgba(255, 255, 255, 0) 0%,
      rgba(255, 255, 255, 0.35) 30%,
      rgba(255, 255, 255, 0.6) 40%,
      rgba(255, 255, 255, 0.8) 60%,
      rgba(255, 255, 255, 0.95) 70%,
      #ffffff 100%
  );
```

附上代码：
```html
<template>
    <div class="newsShare">
        <div class="newsShareMain" :class="showAll===true?'showAll':'showSome'">
            <div class="topFloaBlock">
                <img
                    class="logo"
                    src="https://dss2.bdstatic.com/6Ot1bjeh1BF3odCf/it/u=4217756273,752027184&fm=74&app=80&f=JPEG&size=f121,121?sec=1880279984&t=a4ebf88698c50239adcaa199fa4e172b"
                    alt
                />
                <div class="text">
                    <div class="text1">北高峰APP</div>
                    <div class="text2">下周北高峰APP查看更多</div>
                </div>
                <div class="openBtn">立即打开</div>
            </div>
            <div class="title">{{detailData.title}}</div>
            <div class="accountWrap">
                <div class="accountImgAndName">
                    <img :src="detailData.accountAvatar" alt />
                    {{detailData.accountName}}
                </div>
                <div class="accountState">
                    <div class="followBtn followBtn1" v-if="detailData.accountState === 0">+ 关注</div>
                    <div class="followBtn followBtn2" v-else>已关注</div>
                </div>
            </div>
            <div class="timeAndReadnum">
                {{detailData.time}}
                <img src="../../assets/images/person.png" class="iconPerson" alt />
                {{detailData.readNum}}
            </div>
            <div class="content" v-html="detailData.content"></div>
            <div class="hiddenWrap" v-if="showAll===true">
                <div @click="changeShow" class="showAllBtn">收起全文</div>
            </div>
        </div>
        <div class="showAllWrap" v-if="showAll===false">
            <div @click="changeShow" class="showAllBtn">展开全文</div>
        </div>
        <div class="bottomFloaBlock">打开北高峰APP 查看更多精彩资讯</div>
    </div>
</template>
```
```js
<script>
export default {
    data() {
        return {
            comment: "",
            showAll: false,
            nowId: null,
            detailData: {
                title: "中国政府准备惩戒美国一些个人和实体？中方回应",
                accountName: "浙江应急广播",
                accountAvatar:
                    "https://dss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2164724814,1401845036&fm=26&gp=0.jpg",
                accountState: 0,
                time: "52分钟前",
                readNum: 24786,
                content:
                    '<div style="font-size:17px;line-height:30px;">' +
                    '<span style="font-weight:bold;">【环球时报-环球网报道 记者 乌元春】</span>' +
                    "长江网5月24日讯 5月15日武汉集中核酸检测启动以来，截至5月24日，已为900多万居民采样。为尽可能让从未做过核酸检测的居民都参加本次集中检测，5月23日起，武汉各区共设置了231个“查缺补漏”采样点，为之前因各种原因未能采样检测的居民提供补采服务。对老人、残疾人等特殊群体，医务人员上门为他们采样。" +
                    "长江网5月24日讯 5月15日武汉集中核酸检测启动以来，截至5月24日，已为900多万居民采样。为尽可能让从未做过核酸检测的居民都参加本次集中检测，5月23日起，武汉各区共设置了231个“查缺补漏”采样点，为之前因各种原因未能采样检测的居民提供补采服务。对老人、残疾人等特殊群体，医务人员上门为他们采样。" +
                    '<img style="margin:24px 0;" src="https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1143438203,2101264949&fm=26&gp=0.jpg" alt="">' +
                    "5月15日武汉集中核酸检测启动以来，截至5月24日，已为900多万居民采样。为尽可能让从未做过核酸检测的居民都参加本次集中检测，5月23日起，武汉各区共设置了231个“查缺补漏”采样点，为之前因各种原因未能采样检测的居民提供补采服务。对老人、残疾人等特殊群体，医务人员上门为他们采样。" +
                    "长江网5月24日讯 5月15日武汉集中核酸检测启动以来，截至5月24日，已为900多万居民采样。为尽可能让从未做过核酸检测的居民都参加本次集中检测，5月23日起，武汉各区共设置了231个“查缺补漏”采样点，为之前因各种原因未能采样检测的居民提供补采服务。对老人、残疾人等特殊群体，医务人员上门为他们采样。" +
                    '<img style="margin:24px 0;" src="https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1143438203,2101264949&fm=26&gp=0.jpg" alt="">' +
                    "5月15日武汉集中核酸检测启动以来，截至5月24日，已为900多万居民采样。为尽可能让从未做过核酸检测的居民都参加本次集中检测，5月23日起，武汉各区共设置了231个“查缺补漏”采样点，为之前因各种原因未能采样检测的居民提供补采服务。对老人、残疾人等特殊群体，医务人员上门为他们采样。" +
                    "</div>"
            }
        };
    },
    methods: {
        changeShow() {
            this.showAll = !this.showAll;
        }
    }
};
</script>
```
```css
<style lang="scss">
.newsShare {
    position: relative;
    .showAll {
        height: auto;
    }
    .showSome {
        height: calc(100vh-220px);
        overflow: hidden;
    }
    .newsShareMain {
        padding: 32px;
        padding-top: 174px;
        position: relative;
        color: #222222;
        background-color: #feffff;
        .title {
            font-size: 44px;
            line-height: 60px;
            font-weight: bold;
            margin-bottom: 28px;
        }
        .accountWrap {
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 40px;
            .accountImgAndName {
                font-size: 28px;
                line-height: 40px;
                img {
                    width: 40px;
                    height: 40px;
                    border-radius: 20px;
                    margin-right: 4px;
                }
            }
            .accountState {
                height: 100%;
                font-size: 26px;
                line-height: 40px;
                .followBtn {
                    box-sizing: border-box;
                    height: 42px;
                    padding: 2px 10px;
                    border-radius: 12px;
                    line-height: 42px;
                }
                .followBtn1 {
                    color: #ffffff;
                    background-color: #4273ff;
                }
                .followBtn2 {
                    color: #ffffff;
                    background-color: #bbb;
                }
            }
        }
        .timeAndReadnum {
            color: #9b9b9b;
            font-size: 24px;
            line-height: 34px;
            margin: 16px 0 64px 0;
            display: flex;
            align-items: center;
            .iconPerson {
                display: inline-block;
                width: 30px;
                margin: 0 0 0 26px;
            }
        }
        .content {
            margin-bottom: 36px;
        }
        .hiddenWrap {
            padding-bottom: 100px;
            text-align: center;
            .showAllBtn {
                width: 196px;
                height: 56px;
                margin: auto;
                border: 1px solid #4273ff;
                border-radius: 28px;
                color: #4273ff;
                font-size: 26px;
                line-height: 56px;
                background-color: white;
            }
        }
    }
    .topFloaBlock {
        position: fixed;
        width: 686px;
        top: 0;
        height: 80px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-top: 32px;
        padding-bottom: 30px;
        border-bottom: 1px solid #efefef;
        margin-bottom: 32px;
        background-color: white;
        .logo {
            width: 78px;
            height: 78px;
            border-radius: 12px;
        }
        .text {
            flex: 1;
            margin-left: 12px;
            .text1 {
                font-size: 32px;
                color: #000000;
                line-height: 44px;
                font-weight: bold;
            }
            .text2 {
                font-size: 24px;
                color: #9b9b9b;
                line-height: 34px;
            }
        }
        .openBtn {
            width: 132px;
            height: 56px;
            background-color: #4273ff;
            border-radius: 12px;
            color: #ffffff;
            font-size: 26px;
            line-height: 56px;
            text-align: center;
        }
    }
    .bottomFloaBlock {
        position: fixed;
        bottom: 0;
        width: 100%;
        height: 100px;
        background-color: #4273ff;
        color: white;
        font-size: 38px;
        line-height: 100px;
        text-align: center;
    }
    .showAllWrap {
        position: fixed;
        bottom: 100px;
        width: 100%;
        height: 254px;
        box-sizing: border-box;
        padding-top: 134px;
        color: white;
        font-size: 38px;
        line-height: 100px;
        text-align: center;
        background: linear-gradient(
            to bottom,
            rgba(255, 255, 255, 0) 0%,
            rgba(255, 255, 255, 0.35) 30%,
            rgba(255, 255, 255, 0.6) 40%,
            rgba(255, 255, 255, 0.8) 60%,
            rgba(255, 255, 255, 0.95) 70%,
            #ffffff 100%
        );
        .showAllBtn {
            width: 196px;
            height: 56px;
            margin: auto;
            border: 1px solid #4273ff;
            border-radius: 28px;
            color: #4273ff;
            font-size: 26px;
            line-height: 56px;
            background-color: white;
        }
    }
}
</style>
```
