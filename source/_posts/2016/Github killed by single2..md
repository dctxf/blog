---
title: Github killed by single2.
date: Fri Sep 09 2016 01:43:09 GMT+0800 (CST)
updated: Mon May 22 2017 06:13:58 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /Github-killed-by-single2
---

Github 一直使用正常，可是突然有一天，发现不能提交了，很是郁闷。网上找了一圈也没找到原因。无意间发现，原来是 Github 的公钥(ssh public key)过期了.

<!--more-->

错误信息

```
Github killed by single2.
```

解决办法其实很简单，把公钥重新配置一遍即可。

顺便运行下以下命令，确保可用

```
ssh -T -p 443 git@ssh.github.com
```
