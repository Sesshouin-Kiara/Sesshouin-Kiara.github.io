---
title: 在微信公众号中使用微信支付
date: 2020-03-11 10:44:05
categories:
- JavaScript
tags:
- JavaScript
thumbnail: https://s2.ax1x.com/2019/12/31/l3AAeK.jpg
---
以前做过微信小程序的支付，但是小程序的环境是微信内部提供的,能使用一些微信官方提供的便利的API，而公众号其实是普通的H5网页，两者在代码层面还是有差别的。
<!-- more -->

<div style="text-align:center;"><img src="https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_4_1.png"></div>
<div style="text-align:center;margin-bottom:18px;">微信支付流程图</div>  

* **前提**：公众号需要在微信公众平台中开通支付功能。  


* **第一步**：进入页面时对页面进行重定向，获取<span class="backgroundBlock">code</span>
```javascript
export function redirectPage(path) {
    let appid = "wxa2b43f80deee74aa";
    const url = `http://ylhtest.adt100.com${path}`;
    console.log(url);
    console.log(window.location.href);
    if (window.location.href.includes("code")) {
        // 包含就不需要重定向  只需要获取code
        console.log(getQueryString("code"));
        return getQueryString("code");
    } else {
        // 对页面进行重定向 以便获取code
        window.location.href = `https://open.weixin.qq.com/connect/oauth2/authorize?appid=${appid}&redirect_uri=${url}&response_type=code&scope=snsapi_base&state=STATE`;
        console.log(window.location.href);
        return "";
    }
}
```
  

* **第二步**：获取code之后，调用后端接口获取<span class="backgroundBlock">openid</span>
```javascript
export function getOpenid(code) {
    return dispatch => {
        alert(2222);
        axios({
        method: "get",
        url: global.url.GET_OPENID,
        params: {
            code: code,
            appId: "wxa2b43f80deee74aa"
        }
        })
        .then(res => {
            console.log(res);
            if (res.data.code === 200) {
            dispatch(wxOpenid(res.data.data));
            }
        })
        .catch(err => {
            console.log("openid获取错误");
        });
    };
}
```
  

* **第三步**：获取支付相关字段信息（商品id、价格 等）和openid，传给后端去进行统一下单和生成签名等操作。这些操作完成后，后端会返回前端调用支付接口所需要的数据，数据结构基本如下：
```javascript
"data": {
    "orderNo": "", //订单编号
    "res": {
        "appId" : "wx2421b1c4370ec43b", //公众号名称，由商户传入
        "timeStamp":"1395712654", //时间戳，自1970年以来的秒数
        "nonceStr" : "e61463f8efa94090b1f366cccfbbb444", //随机串
        "package" : "prepay_id=u802345jgfjsdfgsdg888",
        "signType" : "MD5", //微信签名方式：
        "paySign" : "70EA570631E4BB79628FBCA90534C63FF7FADD89" //微信签名
    }
},
```
  
  
* **第四步**：使用上述参数，调用微信的JSAPI接口请求支付。
```javascript
if (typeof WeixinJSBridge == "undefined") {
    if (document.addEventListener) {
       document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady(res),false);
    } else if (document.attachEvent) {
       document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady(res));
       document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady(res));
    }
} else {
     this.onBridgeReady(res);
}
```
```javascript
onBridgeReady(res) {
    const { appId, timeStamp, nonceStr, package, paySign } = res;
    WeixinJSBridge.invoke(
        'getBrandWCPayRequest', 
        {
            "appId" : appId, //公众号名称，由商户传入
            "timeStamp":timeStamp, //时间戳，自1970年以来的秒数
            "nonceStr" : nonceStr, //随机串
            "package" : package,
            "signType" : "MD5", //微信签名方式：
            "paySign" : paySign //微信签名
        },
        (res) => {
            if(res.err_msg == "get_brand_wcpay_request:ok"){
                console.log("success");
                // 使用以上方式判断前端返回,微信团队郑重提示：
                //      res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
            }else{
                console.log("fail");
            }
        }
    )
},
```
>注意：
>1. WeixinJSBridge内置对象仅在手机端微信的浏览器中存在，在其他浏览器中无效。
>2. 微信支付文档表示，JS API的返回结果 get_brand_wcpay_request:ok 仅在用户成功完成支付时返回。由于前端交互复杂， get_brand_wcpay_request:cancel 或者 get_brand_wcpay_request:fail 可以统一处理为用户遇到错误或者主动放弃，不必细化区分
  

* **结果**：第二步客户端发起支付请求后，微信会验证参数和环境的合法性，验证通过后弹出输入支付密码页面，用户输入密码完成支付。微信会根据实际情况通知商户后台和客户端支付结果【根据返回的信息选择进入成功回调还是失败回调，取消支付同样进入失败（cancel）回调】。
  
>总结：
>公众号和小程序的支付流程基本一致，不过由于环境的差异导致代码层面有些不同，例如小程序中，支付是直接调用wx.requestPayment接口，openid也可以直接通过wx的api拿到。