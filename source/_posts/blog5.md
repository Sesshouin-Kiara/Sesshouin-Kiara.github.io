---
title: 发送手机验证码倒计时
date: 2019-12-31 15:27:00
tags:
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
工作中开发项目时，经常会用到手机验证码登录的功能，这里记录一下发送验证码后按钮倒计时的方法。
<!-- more -->
*HTML*
```html
<div class="p-from-group">
    <div class="p-input-item">
        <input
            placeholder="请输入手机号"
            type="number"
            v-model="phone"
        />
    </div>
    <div class="p-input-item">
        <input
            placeholder="请输入验证码"
            type="text"
        />
        <button
            :disabled="disabled"
            @click="sendSMS"
            class="p-btn"
        >{{codeTxt}}</button>
    </div>
</div>
<div class="p-btn-login">登录</div>
```

*JavaScript*
```javascript
export default {
    data() {
        return {
            phone: '',
            codeTxt: "获取验证码",
            disabled: false,
            MAX_COUNT:10,
            count: 10,
        };
    },
    methods: {
        sendSMS() {
            //手机号正则验证
            if (/^1[3456789]\d{9}$/.test(this.phone)) {
                this.count = this.MAX_COUNT
                this.countDown()                    
                console.log('模拟请求验证码,code:2020')
            } else {
                console.log('请输入正确的手机号！')
            }
        },
        // 发送验证码倒计时
        countDown() {
            this.disabled = true
            this.codeTxt = `已发送（${this.count}s）`
            this.count--
            setTimeout(() => {
                if (this.count > 0) {
                    this.countDown()
                }else if (this.count === 0) {
                    this.disabled = false
                    this.codeTxt = "获取验证码"
                }
            }, 1000);
        },
    }
};
```
效果如下：
<div style="text-align:center;"><img src="/print1.png"></div>
点击发送之后：
<div style="text-align:center;"><img src="/print2.png"></div>