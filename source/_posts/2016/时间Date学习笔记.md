---
title: 时间Date学习笔记
date: Fri Sep 09 2016 18:36:35 GMT+0800 (CST)
updated: Mon May 22 2017 06:12:29 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /时间Date学习笔记
---

Date 类型使用自 UTC（Coordinated Universal Time，国际协调时间）1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期。
Date 类型保存的日期能够精确到 1970 年 1 月 1 日之前或之后的 285 616 年。

<!--more-->

## 创建

创建 date 对象，使用 new 操作符和 Date 构造函数

```
var date = new Date();
```

### 不传参

新创建的对象自动获得当前日期和时间

### 传参

#### Date.parse()

- 月/日/年”，如 6/13/2004;`var date = new Date(Date.parse('6/13/2004'))`
- 英文月名日，年”，如 January,12,2004;`var date = new Date(Date.parse('January,12,2004'))`
- 英文星期几英文月名日年时：分：秒时区”，如 Tue May 25 2004 00:00:00 GMT-0700。`var date = new Date(Date.parse('Tue May 25 2004 00:00:00 GMT-0700'))`
- ISO 8601 扩展格式 YYYY-MM-DDTHH:mm:ss.sssZ（例如 2004-05-25T00:00:00）。只有兼容 ECMAScript 5 的实现支持这种格式。`var date = new Date(Date.parse('2004-05-25T00:00:00'))`

如果传入 Date.parse()方法的字符串不能表示日期，那么它会返回 NaN。实际上，如果直接将表示日期的字符串传递给 Date 构造函数，也会在后台调用 Date.parse(),即

```
var date = new Date('6/13/2004');

// 等价于

var date = new Date(Date.parse('6/13/2004'));
```

#### Date.UTC()

Date.UTC()的参数分别是

- **年份(必须)**
- **月份(必须)**（0-11）
- 天（1-31，默认 1）
- 小时数（0-23,默认 0）
- 分钟（默认 0）
- 秒（默认 0）
- 毫秒数（默认 0）
