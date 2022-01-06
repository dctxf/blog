---
title: Mac下用命令查看目录[文件夹]内容大小
date: Thu Jan 28 2016 14:52:12 GMT+0800 (CST)
updated: Sat May 20 2017 11:31:14 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /Mac下用命令查看目录-文件夹-内容大小
---

Mac 下输入`ls`命令可以查看当前文件列表，但是没用文件到大小，很不方便。废话不多说，直接上代码。

<!-- more -->

## 把命令存为 alias

### 已安装 oh my zsh

依次输入以下命令

```
vim ~/.zshrc
```

添加以下代码到`.zshrc`里面

```
alias ldu='ls -1 | xargs du -h -d 0 2>/dev/null'
```

输入以下命令让配置生效

```
source ~/.zshrc
```

### 未安装 zsh

将代码保存到`.bashrc`

到现在为止你就可以用`ldu`来查看文件大小了。
