---
title: Nginx无权限导致的403问题
date: Sat Mar 17 2018 00:57:46 GMT+0800 (CST)
updated: Wed Mar 21 2018 14:04:05 GMT+0800 (CST)
comments: 1
categories: code
tags: [nginx, 403]
permalink: /nginx-403
---

每次新建服务器老是忘记，做个记录。

<!-- more -->

## 问题

安装新的 Nginx 后，有可能网站会出现 404 无权限问题

<!-- more -->

## 原因

Nginx 用户没有 root 权限 导致没有文件访问权限

## 解决办法

修改`nginx.conf`

```nginx.conf
user nginx;
改为
user root;
```
