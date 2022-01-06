---
title: chrome禁止https跳转
date: Sat Mar 04 2017 02:56:29 GMT+0800 (CST)
updated: Mon May 22 2017 06:11:55 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /chrome禁止https跳转
---

最近使用 nginx 配置本地代理，进行本地测试的时候，发现 http 网站总会跳转到 https，很是苦恼，网上找了很多办法发下下面这种办法还是有效的。

<!--more-->

## 原因

chrome 升级最新版后，为了安全考虑，如果站点支持 https，会强制跳转到 https。并且 chrome 会记录 dns，下次访问会直接重定向到 https 站点。

## 解决思路

知道原因之后，下面就是解决办法，删除 chrome 的 dns 记录就行了。

## 具体步骤

```
chrome://net-internals/
```

页面右端红条上有个黑色的下三角

![如图](1.png)

```
clear cache
flush sockets
```

清除两项即可

![如图](2.png)
