---
title: 前后分离之Mock数据
date: Wed Jun 05 2019 17:56:24 GMT+0800 (CST)
updated: Wed Jun 05 2019 17:56:52 GMT+0800 (CST)
comments: 1
categories:
tags: [前端, 前后分离, mock, vue]
permalink: /mock
---

公司做项目自从前后分离之后，接口联调成了最耗时的事情。为了解决这个问题尝试下 mock 数据。

采用了以下技术

- [mockjs](http://mockjs.com/)
- [json-server](https://github.com/typicode/json-server)

由于我们用的前端框架为 vue，这里就用 vue 做例子了

<!-- more -->

## 项目结构

基本架构是`vue-cli3`的基本架构，相比前者多了`mock`文件夹

```
|- mock
    |- api // 动态mock数据
        |- news.json
    |- data // 静态mock数据
        |- user.json
    |- mock.js // mockjs
    |- routes.js // 路由重写
    |- server.js // mock服务
|- public
|- src
...
```

### server.js

```js
const jsonServer = require("json-server");
const server = jsonServer.create();
const mock = require("./mock"); // 动态数据
const routes = require("./routes");
// Support middleware
const middleware = jsonServer.defaults();
server.use(middleware);

const _ = require("underscore");
const path = require("path");
const fs = require("fs");
const mockDir = path.join(__dirname, "data");
// 支持动态数据 mockjs 数据
const base = mock();
// 支持多个静态数据 json文件
const files = fs.readdirSync(mockDir);
files.forEach(function (file) {
  _.extend(base, require(path.resolve(mockDir, file)));
});
const router = jsonServer.router(base);
// 增加路由重写
const rewriteRoutes = jsonServer.rewriter(routes);
server.use(rewriteRoutes);
// 添加自定义路由
server.use(router);

// 返回自定义格式数据
router.render = (req, res) => {
  res.jsonp({
    data: res.locals.data,
    errcode: 200,
    errmsg: "",
  });
};
// 启动服务
server.listen(9090, () => {
  console.log("JSON Server is running");
});
```

### routes.js

```js
module.exports = {
  "/mock/*": "/$1",
  "/posts/:id": "/posts",
  "/blog/:resource/:id/show": "/:resource/:id",
};
```

### mock.js

```js
// 用mockjs模拟生成数据
const Mock = require("mockjs");

const _ = require("underscore");
const path = require("path");
const fs = require("fs");
const mockDir = path.join(__dirname, "api");
// 支持动态数据 mockjs 数据
const base = {};
// 支持多个静态数据 json文件
const files = fs.readdirSync(mockDir);
files.forEach(function (file) {
  _.extend(base, require(path.resolve(mockDir, file)));
});
// mock 数据
const mock = Mock.mock(base);

module.exports = () => {
  return mock;
};
```

### data 静态 mock 数据

以`user.json`为例

```json
{
  "user": {
    "id": 890,
    "name": "演示001",
    "status": 1
  }
}
```

### api 动态 mock 数据

以`course.json`为例

```json
{
  "course|227": [
    {
      "id|+1": 1000,
      "course_name": "@ctitle(5,10)",
      "autor": "@cname",
      "college": "@ctitle(6)",
      "category_Id|1-6": 1
    }
  ]
}
```

## package.json 配置

增加`npm scripts`

```json
"scripts": {
    "mock": "node mock/server.js"
}
```

## vue.config.js 配置

```js
 devServer: {
    proxy: {
      '/mock': {
        target: 'http://localhost:9090',
        changeOrigin: true
      }
    }
  }
```

## 常用占位符

```
"自然数": "@natural",
"浮点数": "@float",
"日期": "@date",
"时间": "@time",
"标题": "@title",
"中文名字": "@cname",
"网址": "@url",
"域名": "@domain",
"邮箱": "@email",
"段落": "@paragraph",
"句子": "@sentence"
```
