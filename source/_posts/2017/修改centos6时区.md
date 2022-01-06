---
title: 修改centos6时区
date: Tue May 23 2017 02:36:29 GMT+0800 (CST)
updated: Tue May 23 2017 02:36:29 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /change-centos6-datezone
---

# 修改 centos6 时区

在写文章的时候发现文章的时间都已经下午了，而我明明是在早上写的，突然想到会不会是时区问题。

<!--more-->

## 时区的相关命令

```bash
date -R	# 显示完整时间+时区
date +%z # 只显示时区

# 修改时区 到 上海 即东八区
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

时区的信息存在`/usr/share/zoneinfo/`下面，本机的时区信息存在`/etc/localtime`

## 拓展阅读

- [CentOS 6 时间，时区，设置修改及时间同步](http://blog.csdn.net/kimsoft/article/details/8091734)
