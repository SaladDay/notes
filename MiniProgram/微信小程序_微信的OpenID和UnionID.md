---
title: 微信的OpenID和ＵｎｉｏｎＩＤ
date: 2020-11-05 16:59:52
tags:
categories: 微信小程序
cover:
---

### **OpenID和unionid**

OpenID：为了识别用户，每个用户针对每个公众号或小程序等应用会产生一个安全的OpenID，公众号或应用可将此ID进行存储，便于用户下次登录时辨识其身份，或将其与用户在第三方应用中的原有账号进行绑定
`UnionId`:UnionId也是用户的标识符，但它与OpenID不同的是，同一个微信用户，登录同一个开发主体下的多个小程序或公众号的时候，分配的UnionId是一样的。

> 可以打个通俗的比喻，比如某个用户是万达商场的会员，那么UnionId就是这个用户对应的万达会员标识，而这个用户又比较爱买买买，在万达商场的很多商店下面都是会员，那么此时每个商店赋予这个用户的会员号就相当于OpenID了，总而言之，在同一个开发体下，UnionId是唯一的，而OpenID不一定唯一。

### **微信授权登录**

整个过程可以总结成以下步骤:

1. 引导进入授权页
   用户授权需要进入以下页面（页面链接参数解释如上表所示）：
   `https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE& state=STATE#wechat_redirect`
2. 用户同意授权(scope=snsapi_base无此步骤)
3. 授权后成功后服务器将code回传回来并重定向到第一步配置的回调页面
4. 微信公众号通过code向微信服务器请求openId
5. 微信公众账号通过Access Token向服务器请求用户信息(scope为snsapi_base时无此步骤)
6. 服务器将用户信息回送给微信公众账号(scope为snsapi_base时无此步骤)
7. 微信公众号通过openId向本服务器请求用户信息（若此openId已与第三方账号有所关联，那么可依据openId查询到用户在第三方数据库中的信息）

![image-20221105170129662](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221105170129662.png)
