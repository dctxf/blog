---
title: window对象的top、parent、opener含义
date: Mon Jan 11 2016 11:38:59 GMT+0800 (CST)
updated: Sat May 20 2017 11:31:27 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /window对象的top、parent、opener含义
---

经常使用 window 对象，是否了解他的 top、parent、opener、self 属性呢？今天就来一探究竟

<!-- more -->

[TOC]

## 介绍

### top

永远指分割窗口最高层次的浏览器窗口。如果计划从分割窗口的最高层次开始执行命令，就可以用 top 变量。

### opener

用于在`window.open`的页面引用执行该`window.open`方法的的页面的对象。

例如：A 页面通过 window.open()方法弹出了 B 页面，在 B 页面中就可以通过 opener 来引用 A 页面，这样就可以通过这个对象来对 A 页面进行操作。

### parent

用于在 iframe,frame 中生成的子页面中访问父页面的对象。

例如：A 页面中有一个 iframe 或 frame，那么 iframe 或 frame 中的页面就可以通过 parent 对象来引用 A 页面中的对象。这样就可以获取或返回值到 A 页面中。

### self

指的是当前窗口

## parent 与 opener 的区别

### parent

parent 指父窗口，在 FRAMESET 中，FRAME 的 PARENT 就是 FRAMESET 窗口。

### opener

- opener 指用 WINDOW.OPEN 等方式创建的新窗口对应的原窗口。
- parent 是相对于框架来说父窗口对象
- opener 是针对于用 window.open 打开的窗口来说的父窗口，前提是 window.open 打开的才有

```
document.parentWindow.menthod()
```

調用父頁面的方法

## Window 对象、Parent 对象、Frame 对象、Document 对象和 Form 对象的阶层关系

```
Window对象→Parent对象→Frame对象→Document对象→Form对象，
```

如下：

```
parent.frame1.document.forms[0].elements[0].value;
```
