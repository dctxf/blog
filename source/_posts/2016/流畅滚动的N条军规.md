---
title: 流畅滚动的N条军规
date: Thu Jan 28 2016 14:55:29 GMT+0800 (CST)
updated: Sat May 20 2017 11:31:09 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /流畅滚动的N条军规
---

网上看大神的视频做了下小笔记，挺实用的。希望对大家有所帮助，**注意：是移动端的开放喔！**

<!-- more -->

1. body 上加上`-webkit-overflow-scrolling:touch`
2. iOS 尽量使用局部滚动
3. iOS 引进 ScrollFix 避免出界
4. Android 下尽量使用全局滚动

5. 尽量不用`overflow:auto`
6. 使用`min-height:100%`代替`height:100%`

7. iOS 下带有滚动条且`position:absoulte`的节点不要设置背景色
