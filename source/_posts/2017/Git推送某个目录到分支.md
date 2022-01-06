---
title: Git推送某个目录到分支
date: Thu Jun 15 2017 10:12:57 GMT+0800 (CST)
updated: Fri Jun 23 2017 08:25:23 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /git-push-some-folder-to-subtree
---

很多 GIT 平台都支持静态页面展示功能，但是项目中到静态页面都是自动生成的，如何利用 pages 功能来展示静态页面呢？我的做法如下。

<!-- more -->

## 步骤

1. 生成静态页面到`dist`目录
2. 推送`dist`目录到`dist`分支
3. 给`dist`分支开启`pages`

## 具体操作

生成`dist`目录及文件，运用`git subtree`推送到`dist`分支

```bash
git subtree push --prefix dist origin dist
```

## 拓展阅读

- [用 Git Subtree 在多个 Git 项目间双向同步子项目，附简明使用手册](http://tech.youzan.com/git-subtree/)
