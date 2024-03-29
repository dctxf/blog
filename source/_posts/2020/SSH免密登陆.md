---
title: SSH免密登陆
date: 2020-03-31 13:34:37
tags: [ssh]
---

经常用 ssh 连接服务器，这里主要记下怎么免密码登录

<!-- more -->

## 连接服务器

```
ssh root@127.0.0.1 -p 2222
# ssh 用户@host -p 端口
```

## 免密码登录

### 用 ssh-keygen 创建公钥

在客户端创建公钥

`如提示输入密码可忽略`

```bash
ssh-keygen -t rsa

<!-- 你会看到以下代码 -->
Generating public/private rsa key pair.
Enter file in which to save the key (/home/haifeng/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/haifeng/.ssh/id_rsa.
Your public key has been saved in /home/haifeng/.ssh/id_rsa.pub.
The key fingerprint is:
7b:75:98:eb:fd:13:ce:0f:c4:cf:2c:65:cc:73:70:53 haifeng@haifeng-EX38-DS4
The key's randomart image is:
+--[ RSA 2048]----+
|                E|
|                .|
|              ...|
|             + =.|
|        S   + +.*|
|         . . + Bo|
|        . . . = =|
|         . . . * |
|            . ..=|
+-----------------+
```

### 查看

```bash
ls ~/.ssh/
# id_rsa  id_rsa.pub  known_hosts
```

### 上传公钥

在服务器上创建`authorized_keys`并**修改权限**

```bash
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

**注意：** 必须将`~/.ssh/authorized_keys`的权限改为 600

```bash
scp ~/.ssh/id_rsa.pub root@127.0.0.1:~/.ssh/authorized_keys
# scp 本地公钥 user@host:~/.ssh/authorized_keys
```

## 拓展阅读

- [SSH 原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
- [SSH 原理与运用（二）：远程操作与端口转发](http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html)
