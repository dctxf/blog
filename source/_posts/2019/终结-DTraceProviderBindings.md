---
title: 终结 DTraceProviderBindings
date: 2019-08-09 08:47:53
tags: [mac, hexo, node]
---

使用 hexo 的时候老是出现找不到`./build/Release/DTraceProviderBindings`这个包。真是给跪了。Google 了很多答案，发现都没什么用。下面就讲下我的解决办法，希望对你也有用。

<!--more-->

## 方法一

```shell
# 先卸载已安装的hexo
npm uninstall hexo-cli hexo -g
# 确保/usr/local/lib/node_modules/ 没有 hexo-cli
ls /usr/local/lib/node_modules/
# 确保 /usr/local/bin/ 没有 hexo
ls /usr/local/bin/ | grep hexo
# 再次安装
npm install hexo-cli hexo -g
# 检测是否有错
hexo -v
```

## 方法二

此法网上说的最多，但也没什么卵用

```bash
npm install hexo --no-optional
```

## 方法三

这个是我的方法，我在无意中发现，我的`package.json`中有两个`hexo`的依赖，我删除了`devDependencies`中的依赖，然后就没问题了

具体做法如下

```bash
vim package.json
```

删除下面的东西

```json
"devDependencies": {
    "hexo": "^3.1.0" # 如果有，删除此行
  }
```

如果不放心可运行以下命令

```bash
# 删除已安装的包
rm -rf node_modules
# 重新安装依赖
npm i
```

以上就是解决办法。希望对你有用。
