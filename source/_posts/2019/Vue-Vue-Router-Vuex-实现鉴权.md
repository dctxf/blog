---
title: Vue+Vue Router+Vuex 实现鉴权
date: 2019-08-09 16:51:28
tags: [vue, vue router, vuex, 鉴权]
---

工作中采用的前端框架为 vue，项目中经常会碰到鉴权问题，比如某个页面必须登录了才能用，或者必须提前获取到某些参数才行。这时候就需要进行拦截处理

<!--more-->

## 原理

1. 用户进入页面
2. 判断是否符合条件，如果不符合跳转到对应页面进行补充

## 主要代码

首先需要在`router`里面进行拦截，这里采用 router 的`beforeEach`

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import store from '../store'
import _ from 'lodash'
import { getUserInfo } from '@/service'

Vue.use(VueRouter)

const router = new VueRouter({
  base: process.env.BASE_URL,
  routes: [
    {
      path: '/',
      name: 'Home',
      redirect: {
        name: 'Login'
      },
      meta: {
        title: '首页'
      }
    },
    {
      path: '/user',
      name: 'User',
      meta: {
        title: '首页',
        auth: ['user']
      }
    }
  ]
})

router.beforeEach(async (to, from, next) => {
  if (to.meta.title && to.meta.title !== from.meta.title) {
    document.title = to.meta.title
  }

  const { state, dispatch } = store
  // 获取授权列表 并去重
  let authList = []
  to.matched.map(record => {
    const auth = record.meta.auth
    if (auth) {
      authList = _.union(authList, auth)
    }
  })
  for (let index = 0; index < authList.length; index++) {
    const key = authList[index].toLocaleLowerCase()
    // 获取用户信息
    if (key === 'user' && !state.userInfo.id) {
      try {
        const user = await getUserInfo()
        dispatch('initUser', user)
      } catch (e) {
        next({
          replace: true,
          path: '/login',
          query: {
            redirect: to.fullPath
          }
        })
      }
    }
  }
  next()
})
```
