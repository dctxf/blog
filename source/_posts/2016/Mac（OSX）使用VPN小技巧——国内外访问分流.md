---
title: Mac（OSX）使用VPN小技巧——国内外访问分流
date: Thu Jan 07 2016 13:16:23 GMT+0800 (CST)
updated: Sat May 20 2017 11:34:34 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /Mac（OSX）使用VPN小技巧——国内外访问分流
---

自从 Mac 用上了 VPN，科学的上网让我的小 Mac 充满了活力。巴特，也带来了一个大问题，开启 VPN 的时候访问国内网站有时候特别慢。难道要我每次访问国内都要断开，访问国外再连上，接受无能啊！！！实际上，大部分 windows 的 VPN 客户端软件，都集成国内外分流的功能，非常便捷，而 Mac 上目前还没有类似的软件有此功能，好吧，只能自己动手，丰衣足食了。

<!--more-->

## 为什么要分流

> 自从 Mac 用上了 VPN，科学的上网让我的小 Mac 充满了活力。巴特，也带来了一个大问题，开启 VPN 的时候访问国内网站有时候特别慢。难道要我每次访问国内都要断开，访问国外再连上，接受无能啊！！！实际上，大部分 windows 的 VPN 客户端软件，都集成国内外分流的功能，非常便捷，而 Mac 上目前还没有类似的软件有此功能，好吧，只能自己动手，丰衣足食了。

## Mac 下国内外访问分流有啥好处？

- 连上 VPN 以后，不会影响访问国内网站的速度
- 有些 VPN 提供的是流量套餐，如果不分流，一个月流量伤不起啊

##如何实现 Mac 下 VPN 的国内外分流？
Mac 下的分流方法来源于 chnroute 这个项目，对作者表示衷心的感谢
项目地址：https://github.com/jimmyxu/chnroutes
此项目不仅仅是针对 Mac，而且同时支持 windows/linux，以及基于 linux 的路由器。

## Mac 下的使用步骤：

1. 首先，你要弄一个 VPN 并且设置好 PPTP，免费 VPN 之前我介绍过一个合集，可以看这里;
2. 下载`chnroutes.py`这个文件，比如保存到下载里面，[下载地址](https://chnroutes.googlecode.com/files/chnroutes.py)
3. 打开终端进入下载文件所在的目录，执行`python chnroutes.py -p mac`，在该目录下会生成 2 个文件，`ip-up`和`ip-down`；

```
macbookair:~ Peter$ cd Downloads
macbookair:Downloads Peter$ python chnroutes.py -p mac
Fetching data from apnic.net, it might take a few minutes, please wait...
For pptp on mac only, please copy ip-up and ip-down to the /etc/ppp folder,don't forget to make them executable with the chmod command.
```

4. 打开 Finder 进入下载，可以找到刚刚生成的 2 个文件，选中并复制这 2 个文件；按下快捷键`command+shift+g`，弹出的地方输入`/etc/ppp`，进入此目录，粘贴；
5. 回到终端，进入目录`/etc/ppp`执行：`sudo chmod a+x ip-up ip-down`

```
macbookair:Downloads Peter$ cd /etc/ppp
macbookair:ppp Peter$ sudo chmod a+x ip-up ip-down
```

（这一步可能会提示输入系统密码）
好了，Done！连上 VPN 测试一下吧。

## 如何测试是否成功？

比较简单的测试方法是打开 2 个 ip 地址查询网站。如果一切正常，连上 vpn 以后，打开 ip138，会显示你本来的 ip 地址（你还在国内），打开这个网站，会显示 vpn 服务器的 ip 地址（你到国外了）。

高端一点的，可以在终端输入`traceroute www.baidu.com`以及`traceroute www.youtube.com`，看看访问国内网站和国外网站的路由情况。

## 如果我不想要分流了，怎么破？

直接把`/etc/ppp`下面那 2 个文件删了就行了。
