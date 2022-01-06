---
title: 利用Nginx和Host把自定义域名指向本地
date: Tue Sep 06 2016 16:28:38 GMT+0800 (CST)
updated: Sat May 20 2017 11:29:15 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /利用Nginx和Host把自定义域名指向本地
---

假如有域名 a.com，怎么把它和自己的本地目录做链接呢？今天玩 Nginx 的时候终于知道怎么玩了。简单记录一下。

<!--more-->

## 原理

- 利用 hosts 把域名指向本地即 127.0.0.1
- 利用 nginx 进行域名跳转和目录指定

## 实施

### 安装 nginx

```
brew install nginx
```

修改配置

```
sudo vim /usr/local/etc/nginx/nginx.conf
#修改默认的8080端口为80
```

如果你开启了 apache，可能会造成 403，把 apache 关了就行

给予管理员权限

```
sudo chown root:wheel /usr/local/opt/nginx/bin/nginx
sudo chmod u+s /usr/local/opt/nginx/bin/nginx
```

加入 launchctl 启动控制

```
mkdir -p ~/Library/LaunchAgents
cp /usr/local/opt/nginx/homebrew.mxcl.nginx.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
```

运行 nginx

```
sudo nginx #打开 nginx
nginx -s reload|reopen|stop|quit  #重新加载配置|重启|停止|退出 nginx
nginx -t   #测试配置是否有语法错误
```

用法详解

```
nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]
```

选项列表

```
-?,-h           : 打开帮助信息
-v              : 显示版本信息并退出
-V              : 显示版本和配置选项信息，然后退出
-t              : 检测配置文件是否有语法错误，然后退出
-q              : 在检测配置文件期间屏蔽非错误信息
-s signal       : 给一个 nginx 主进程发送信号：stop（停止）, quit（退出）, reopen（重启）, reload（重新加载配置文件）
-p prefix       : 设置前缀路径（默认是：/usr/local/Cellar/nginx/1.2.6/）
-c filename     : 设置配置文件（默认是：/usr/local/etc/nginx/nginx.conf）
-g directives   : 设置配置文件外的全局指令
```

## 配置

以上都完成之后，浏览器输入 localhost 如果能够正常访问证明就可以了。

接下来进行下一步的配置

找到 nginx 的配置目录，**不同的版本可能不太一样**
我的是在`/usr/local/etc/nginx`，进入`servers`目录，有的可能叫`conf.d`，新建你的配置文件

```
vim demo.conf
```

编辑配置文件

```
server {
  listen 80;
  server_name a.com;
  root /Users/dctxf/Desktop/demo;
}
```

浏览器输入 a.com 是不是能访问了。

## 问题

### mac hosts 文件不生效

在学习过程中发现 hosts 文件不生效，必须重启，后来才发现**重启网络服务就行**，具体什么原因还不是很清楚，网上有说是因为浏览器的长链接，关闭浏览器或者隐身模式访问，我试了下还是不行。因为我用了 surge，发现重新加载 surge 的配置文件就行，不清楚具体原因。

### nginx 命令不能用

输入 nginx 命令后会提示无此命令，应该是环境变量的问题，我的解决办法是修改了.zshrc 的配置

```
export PATH=${PATH}:/usr/local/Cellar/nginx/1.10.1/bin
```
