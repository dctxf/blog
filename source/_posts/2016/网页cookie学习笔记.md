---
title: 网页cookie学习笔记
date: Fri Jan 08 2016 10:31:31 GMT+0800 (CST)
updated: Sat May 20 2017 11:33:24 GMT+0800 (CST)
comments: 1
categories:
tags: []
permalink: /网页cookie学习笔记
---

学习 cookie 的一些小笔记，希望给大家点帮助

<!-- more -->

```
/**
 * [setCookie 设置cookie]
 * @param {[type]} name  [名字]
 * @param {[type]} value [值]
 * @param {[type]} day   [过期时间]
 */
function setCookie(name , value , day){
  var oDate = new Date();

  oDate.setDate(oDate.getDate() + day); // 设置过期时间

  document.cookie = name + '=' + value + ';expires=' + oDate;

}

/**
 * [getCookie 获取cookie]
 * @param  {[type]} name [名字]
 * @return {[type]}      [返回值无则返回空]
 */
function getCookie(name){
  var arr = document.cookie.split('; ');
  var i = 0;
  for(;i < arr.length;i++){
    var arr2 = arr[i].split('=');
    if (arr2[0] == name) {
      return arr2[1];
    }
  }
  return '';
}

/**
 * [removeCookie 删除cookie]
 * @param  {[type]} name [名字]
 */
function removeCookie(name){
  setCookie(name,'',-1);
}

```

> firefox 支持本地查看 cookie，其他浏览器需要启动 http 服务
