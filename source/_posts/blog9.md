---
title: Ant Design of Vue 的表单自定义验证规则
date: 2020-01-08 14:32:57
categories:
- [JavaScript]
- [Vue]
- [Ant Design]
tags:
- JavaScript
- Vue
- Ant Design
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
公司的后台项目使用的UI库是Ant Design。在工作中，由于 Ant Design 表单 From 提供的验证方式比较单一，经常会用到表单Form的自定义验证，这里总结一下基本的用法。
<!-- more -->
```javascript
<a-input
    type="number"
    v-decorator="['saleCost', { rules: [
        { required: true, message: '请输入金额！'},
        { validator: validatorCost }
    ]}]"
/>

methods: {
    validatorCost(rule, value, callback) {
        var reg = /^(?:0\.\d{0,1}[1-9]|(?!0)\d{1,8}(?:\.\d{0,1}[1-9])?)$/;
        if (!value||value === '') {
            // 不填写金额时，希望走 required: true 的验证，所以自定义验证这边要通过，不然会报两次
            callback();
            // 如果去掉 required: true 的验证，则这里可以写 callback('请输入金额！');
        } else if (!reg.test(value)) {
            callback('请输入正确的金额格式！');  //验证不通过
        } else if (value > 10000) {
            callback('金额不得超过10000！');  //验证不通过
        } else {
            callback();  //验证通过
        }
    },
}
```