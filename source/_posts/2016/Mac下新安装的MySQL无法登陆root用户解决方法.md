---
title: Mac下新安装的MySQL无法登陆root用户解决方法
date: Thu Jan 28 2016 07:02:57 GMT+0800 (CST)
updated: Sat Mar 04 2017 02:13:53 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /Mac下新安装的MySQL无法登陆root用户解决方法
---

也不知是何原因，新安装好的 MySQL，如果尝试用`mysql -u root -p`登陆就会出现这样的错误，但是 root 用户根本就没有设置密码。
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

<!-- more -->

网上搜了很多，最后发现原来是 mysql 升级了，安装的时候需要注意下，以前 mysql 的 root 用户密码为 root，新版安装则会生成一个随机密码，一定要记得这个秘密。如果你折腾了很久，建议把 mysql 删除重新安装，**记好安装时候弹出的秘密喔！**

这样再次按照正常流程输入密码就行了。
