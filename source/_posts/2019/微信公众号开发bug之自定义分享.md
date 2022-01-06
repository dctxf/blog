---
title: 微信公众号开发bug之自定义分享
date: Mon May 27 2019 09:13:46 GMT+0800 (CST)
updated: Mon May 27 2019 09:14:01 GMT+0800 (CST)
comments: 1
categories:
tags: [微信, 微信开发, 公众号, bug]
permalink: /mp-bug-diy-share
---

最近在做微信公众号自定义分享时碰到一个问题，就是自定义分享的时候有时候成功有时候失败，大部分都是失败。这里总结下自己解决的方法

<!-- more -->

## 前期准备

1. 申请好的微信公众号账号
2. 先登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS 接口安全域名”。
3. 页面已引入微信官方的 JSSDK，由于 SDK 版本升级了，可以引入新版本的 SDK，但是微信官方工具竟然不支持调试
4. 通过 config 接口注入权限验证配置 这里可通过后台接口返回相应的配置
5. 通过 ready 接口处理成功验证 分享的代码要写在这部分
6. 通过 error 接口处理失败验证 如果第一次配置失败了，还可以通过此接口再配置一次

## 注意

基本上通过上面的配置就能实现自定义分享了，但是其实还有一些细节需要注意

1. 上面第 4 步中
   1. 如果 SDK 是老版本配置如下 `jsApiList:['onMenuShareTimeline','onMenuShareAppMessage','onMenuShareQQ','onMenuShareQZone']`
   2. 如果 SDK 为 1.4.0 以上版本 `jsApiList:['updateAppMessageShareData','updateTimelineShareData']`
2. 第 5 步中的配置如下

   ```
   const config = {
           title: '', // 分享标题
           desc: '', // 分享描述
           link: '', // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
           imgUrl: '', // 分享图标
           success: function () {
             // 设置成功
           }
   }
   wx.ready(function () {   //需在用户可能点击分享按钮前就先调用
       // 老版本写法
       wx.onMenuShareTimeline(config)
       wx.onMenuShareAppMessage(config)
       wx.onMenuShareQQ(config)
       wx.onMenuShareQZone(config)
       // 新版本
       wx.updateAppMessageShareData(config)
       wx.updateTimelineShareData(config)
   });
   ```

   这里需要注意的是

   - `link`中的分享链接如果当中包含中文一定要进行`encode`
