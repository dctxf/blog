---
title: 博客从hexo到firekylin遇到的问题
date: Tue May 23 2017 02:56:24 GMT+0800 (CST)
updated: Tue May 23 2017 11:41:30 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /blog-from-hexo-to-firekylin
---

我的博客很早是听朋友说 hexo，静态博客很吊，于是就用了，但是发现自己学习不了什么东西，于是就想能不能换个动态的。当然最早的就是 wp，但是发现速度很慢。这时有听朋友说 Thinkjs（所以说多跟朋友聊聊天，哈哈），有个 firekylin 博客，基于 nodejs，自己至少能看懂些。所以就转到这里了。但是问题又来了，这个博客竟然没有个好看的主题...作为(前)设计师怎么能忍。于是就有了以下。

<!--more-->

## 使用线上数据

代码上传到`git`上，再从别的地方`clone`下来，需要重新安装数据库。能不能直接用呢。查看 firekylin 的源码之后发现。线上和本地的差别在于两个文件

### .installed

是否安装

```
firekylin
```

### `src/common/config/db.js`

数据库配置

```javascript
"use strict";
exports.__esModule = true;
exports.default = {
  type: "mysql",
  adapter: {
    mysql: {
      host: "localhost",
      port: "3306",
      database: "fk_blog",
      user: "root",
      password: "123456",
      prefix: "fk_",
      type: "mysql",
    },
  },
};
```

## 解决 nunjucks 纯文本输出

firekylin 使用了 nunjucks 作为渲染模板，但是在渲染文章简介的时候老是会把 html 标签给带过来一起给渲染了。我这里的解决办法是，给 nunjucks 添加自定义过滤器。

找到`src/common/config/view.js`并修改

```javascript
"use strict";

import { parse } from "url";

const build_query = (obj) =>
  "?" +
  Object.keys(obj)
    .map((k) => encodeURIComponent(k) + "=" + encodeURIComponent(obj[k]))
    .join("&");
/**
 * template config
 */
export default {
  type: "nunjucks",
  content_type: "text/html",
  file_ext: ".html",
  file_depr: "_",
  root_path: think.ROOT_PATH + "/view",
  adapter: {
    nunjucks: {
      prerender: function (nunjucks, env) {
        env.addFilter("utc", (time) => new Date(time).toUTCString());
        env.addFilter("pagination", function (page) {
          let { pathname, query } = parse(this.ctx.http.url, true);

          query.page = page;
          return pathname + build_query(query);
        });
        // 添加自定义过滤器 提取纯文本
        env.addFilter("text", function (str) {
          let reg = /<[^>]*>/g; // 过滤html标签正则
          return str.replace(reg, ""); // 返回纯文本
        });
      },
    },
  },
};
```

## 解决后台编译错误不能显示

不知道为毛，把代码复制下来之后，后台死活不能显示。线上正常。

**突然灵机一动，是不是因为线上用的`npm`安装依赖，本地用的`cnpm`安装依赖。于是果断试下。结果不出所料。**
