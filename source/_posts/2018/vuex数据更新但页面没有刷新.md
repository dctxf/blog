---
title: vuex数据更新但页面没有刷新
date: Wed Aug 15 2018 16:33:50 GMT+0800 (CST)
updated: Wed Aug 15 2018 16:34:03 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /vuex-update-but-no-refresh-the-page
---

# vuex 数据更新但页面没有刷新

再使用 vuex 的过程中经常遇到，数据虽然更新了，但是页面并没有更新，网上搜了一些解决办法并不能完全解决问题

<!-- more -->

## 原因

由于 JavaScript 的限制， Vue 不能检测变动的数组

## 解决方案

- 官方推荐说如果是异步操作要使用`action`不用使用`mutaion`
- 网上推荐使用 watch 监听数据变化

如果上面两种都不行的话，可以尝试下下面的方案

经测试如果对象层级太深，却是不能及时刷新，但是如果是新对象的话就不会出现这种问题，那么每次重新赋值不久行了 😁

例如

```javascript
// store.js
Const state = {
	user:{}
}
```

```javascript
Const mutations = {
	SET_USER(state,user){
		state.user = {
			name:user.name
		}
		// or
		state.user = cloneDeep(user)
	}
}
```
