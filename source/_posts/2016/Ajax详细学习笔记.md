---
title: Ajax详细学习笔记
date: Mon Jan 11 2016 11:42:32 GMT+0800 (CST)
updated: Sat May 20 2017 11:31:20 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /Ajax详细学习笔记
---

一直以为 ajax 是一个很神奇的东西，好容易在网上找到视频，下面是我学习的一些学习笔记，希望对大家有所帮助。本文从 http 基础说起，相信大家能看懂的，请耐心喔！

<!-- more -->

[TOC]

## HTTP

### 介绍

> 超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是互联网上应用最为广泛的一种网络协议。设计 HTTP 最初的目的是为了提供一种发布和接收 HTML 页面的方法。通过 HTTP 或者 HTTPS 协议请求的资源由统一资源标识符（Uniform Resource Identifiers，URI）来标识。

HTTP 是一种无状态协议。（不建立持久的链接）

[维基百科解释](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)

### 完整 HTTP 请求

1. 建立 TCP 连接
2. Web 浏览器向 Web 服务器发送请求命令
3. Web 浏览器发送请求头信息
4. Web 服务器应答
5. Web 服务器发送应答头信息
6. Web 服务器向浏览器发送数据
7. Web 服务器关闭 TCP 连接

### HTTP 组成

1. HTTP 请求的方法或动作，（例：GET 或 POST）
2. 正在请求的 URL
3. 请求头（包含客户端环境信息，身份验证信息等）
4. 请求体，也是请求正文（包含客户提交的查询字符串信息，表单信息等）

### 请求状态

HTTP 状态码由 3 位数字构成，其中首位数字定义了状态码的类型

#### 1XX

信息类，表示收到 Web 浏览器请求，正在进一步处理中

#### 2XX

成功，表示用户请求被正确接收，理解和处理（例：200 OK）

#### 3XX

重定向，表示请求没有成功，客户必须采取进一步动作

#### 4XX

客户端错误，表示客户端提交的请求有错误（例：404 NOT Found，意味请求中所引用的文档不存在）

#### 5XX

服务端错误，表示服务器不能完成对请求对处理（例：500）

### HTTP 请求

#### GET 请求

- 一般用于信息获取
- 使用 URL 传递参数
- 对发送信息数量有限制，一版在 2000 个字符

#### POST 请求

- 一般用于修改服务器上资源
- 对发送信息数量无限制

### HTTP 响应

1. 一个数字和文字组成的状态码（成功，失败等）
2. 响应头（包含服务器类型，日期时间，内容类型，长度等）
3. 响应体，也是响应正文

## request 对象

### 兼容写法

```
var request;
if (window.XMLHttpRequest) {
  request = new XMLHttpRequest();
} else {
  request = new ActiveXObject('Microsoft.XMLHTTP');
}
```

### 主要方法

#### open(method,url,async)

#### send(string)

### 取得响应

#### responseText

获得字符串形式的响应数据

#### responseXML

获得 XML 形式的响应数据

#### status 和 statusText

以数字和文本形式返回 HTTP 状态码

#### getAllResponseHeader()

获取所有的响应报头

#### getResponseHeader()

查询响应中的某个字段值

#### readyState

会返回 0，1，2，3，4 几个值

0:请求未初始化，open 还没有调用
1:服务器连接已建立，open 已经调用
2:请求已接收，即接收到头信息了
3:请求处理中，即接收到响应主体
4:请求已完成，且响应已就绪，即响应完成

```
request.open('GET', 'get.php', true);
request.send();
request.onreadystatechange = function() {
  if (request.readyState === 4 && request.status === 200) {
    //做一些事情 request.responseText
  }
};
```

## JSON

### 介绍

javascript 对象表示法（javaScript Object Notation）

JSON 是存储和交换文本信息的语法，类似 XML。它采用健值对的方式组织，易于阅读，编写和机器解析，生成

JSON 是独立于语言的，即任何语言都可以解析 JSON，只需要按照 JSON 的规则即可

### 优点

- json 的长度和 xml 相比更小
- json 读写速度更快
- json 可以使用 javascript 内建的方法直接解析，转换成 javascript 对象方便

### 书写格式（名称/值对）

名称/值对组合中的名称写在前面（在双引号中），值对写在后面（同样在双引号中），中间用冒号隔开（例："name":"dctxf"）

### 可用类型

- 数字（整数和浮点数） 例：123 123.123
- 字符串（在双引号中） 例："dctxf"
- 逻辑值（true 或 false） 例：true
- 数组（在方括号中） 例：[1，2，3，4]
- 对象（在花括号中） 例：{}
- null

### 解析方式

- `eval`和`JSON.parse`
- **在代码中使用 eval 是恨危险的！**特别是用它执行第三方的 JSON 数据（其中可能包含恶意代码）时，尽可能使用`JSON.parse()`方法解析字符串本身，该方法还可以捕捉 JSON 中的语法错误。

JSON 字符串

```
var data = '{"fruit": [{"name": "apple","number": 10}, {"name": "orange","number": 12}]}';
```

#### eval

```
var oJson = eval('(' + data + ')');

console.log(oJson.fruit[0].name);
//输出 apple
```

由于`eval`可以解析以下

```
eval('alert(1)');
```

所以尽量不要使用`eval`

#### JSON.parse

```
var oJson = JSON.parse(data);

console.log(oJson.fruit[0].name);
//输出 apple
```

#### 拓展阅读

[拓展阅读](https://zh.wikipedia.org/wiki/JSON)
[json 官网](http://www.json.org/)

## 跨域

- 域名地址组成

![](http://dctxf-open.qiniudn.com/kuayu1.png)

- 当**协议、子域名、主域名、端口号中任意一个不想同时**，都算做跨域
- 不用域之间相互请求资源，就算做**跨域**
- Javascript 出于安全方面考虑，不允许跨域调用其它页面的对象

### 服务器代理

### JSONP

只能对`GET请求起作用`

### XHR2

- html5 提供 XMLHttpRequest Level2 已经实现了跨域访问及其它新功能
- IE10 以下版本不支持
