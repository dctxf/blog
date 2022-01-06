---
title: 数组Array学习笔记
date: Thu Sep 08 2016 17:13:57 GMT+0800 (CST)
updated: Mon May 22 2017 06:41:04 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /数组Array学习笔记
---

“虽然 ECMAScript 数组与其他语言中的数组都是数据的有序列表，但与其他语言不同的是，ECMAScript 数组的每一项可以保存任何类型的数据。也就是说，可以用数组的第一个位置来保存字符串，用第二位置来保存数值，用第三个位置来保存对象，以此类推。而且，ECMAScript 数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。”

<!--more-->

摘录来自: 泽卡斯. “JavaScript 高级程序设计（第 3 版）”。 iBooks.

## 创建

```
var arr = [];
var arr = new Array();

var arr = [1,2,3];
var arr = new Array(1,2,3);

var arr = new Array(3);  // [undefined,undefined,undefined]

var arr = ['red','blue','yellow'];
var arr = new Array('red','blue','yellow');
```

## 检测

```
arr instanceof Array //true 为数组
Array.isArray(arr) //true 为数组
```

## 转换

```
var arr = [1,2,3]
arr.toString();
arr.valueOf();
arr.toLocaleString();
// return 1,2,3
```

toLocaleString()方法经常也会返回与 toString()和 valueOf()方法相同的值，但也不总是如此。当调用数组的 toLocaleString()方法时，它也会创建一个数组值的以逗号分隔的字符串。而与前两个方法唯一的不同之处在于，这一次为了取得每一项的值，调用的是每一项的 toLocale-String()方法，而不是 toString()方法。

## 栈方法&队列方法

### 栈方法

栈是一种 LIFO（Last-In-First-Out，后进先出）的数据结构，也就是最新添加的项最早被移除。

#### push&pop

```
var arr = [1,2];
arr.push(3); //3
alert(arr); //[1,2,3]

arr.pop(); //3
alert(arr); //[1,2]
```

### 队列方法

队列数据结构的访问规则是 LIFO（ First-In-First-Out，先进先出）

#### push&shift

```
var arr = [1,2];
arr.push(3); //3
alert(arr); //[1,2,3]

arr.shift(); //1
alert(arr); //[2,3]
```

#### unshift

E7 及更早版本对 JavaScript 的实现中存在一个偏差，其 unshift()方法总是返回 undefined 而不是数组的新长度。IE8 在非兼容模式下会返回正确的长度值。

```
var arr = [1,2];
arr.unshift(1); //返回length 3
alert(arr); //[1,2,3]
```

## 排序

### reverse

反转数组

```
var arr = [1,2,3];
arr.reverse();
alert(arr); //[3,2,1]
```

### sort

在默认情况下，sort()方法按升序排列数组项一一即最小的值位于最前面，最大的值排在最后面。为了实现排序，sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串

```
var arr = [1,5,10,15];
arr.sort(); // [1,10,15,5];
/*因为数值5虽然小于10，但在进行字符串比较时，"10"则位于"5"的前面，于是数组的顺序就被修改了。*/

function compare(value1,value2){
  if(value1 > value2){
    return -1;
  }else if(value1 < value2){
    return 1;
  }else{
    return 0;
  }
}

arr.sort(compare); //[1,5,10,15]

function compare(value1,value2){
  if(value1 > value2){
    return 1;
  }else if(value1 < value2){
    return -1;
  }else{
    return 0;
  }
}

arr.sort(compare); //[15,10,5,1]
```

## 操作

### concat

复制并返回副本

```
var arr = [1,2];
arr.concat(3,4,5); //[1,2,3,4,5]
alert(arr); //[1,2]
```

### slice

```
var arr = [1,2,3,4];

arr.slice(1) //[2,3,4]
arr.slice(1,3); //[2,3]
arr.slice(-1); //[4]
arr.slice(-1,-3); //[]
arr.slice(-3,-1); //[2,3]
```

### splice

删除

```
var arr = [1,2,3];
arr.splice(1,1); //[1,3]
```

增加

```
var arr = [1,2,3];
arr.splice(1,0,4); //[1,4,2,3]
arr.splice(1,0,'a','b'); //[1,'a','b',4,2,3]
```

插入替换

```
var arr = [1,2,3];
arr.splice(1,1,4); //[1,4,3]
```

## 位置

### indexOf

返回索引,从前到后

```
var arr = [1,2,3];
arr.indexOf(2); //1
```

### lastIndexOf

返回索引,从后到前

```
var arr = [1,2,3];
arr.lastIndexOf(2); //1
```

## 迭代

### every

对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。

```
var arr = [1,2,3];
arr.every(function(item,index,arr){
  return item > 2;
})
// false
```

### some

对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。

```
var arr = [1,2,3];
arr.some(function(item,index,arr){
  return item > 2;
})
// true
```

### filter

对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。

```
var arr = [1,2,3];
arr.filter(function(item,index,arr){
  return item > 2;
})
// [3]
```

### map

对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

```
var arr = [1,2,3];
arr.map(function(item,index,arr){
  return item > 2;
})
// [false,false,true]


arr.map(function(item,index,arr){
  return item * 2;
})
//[2,4,6]
```

### forEach

对数组中的每一项运行给定函数。这个方法没有返回值。

```
var arr = [1,2,3];
arr.forEach(function(item,index,arr){
  item > 2 ? console.log('大于2') : console.log('小于2');
})
//小于2 小于2 大于2
```

## 缩小

`reduce`和`reduceRight`两个方法都会迭代数组的所有项，然后构建一个最终返回的值。

### reduce

```
var arr = [1,2,3];
arr.reduce(function(prev,cur,index,arr){
  return prev + cur;
})
//1+2+3=6
```

### reduceRight

```
var arr = [1,2,3];
arr.reduceRight(function(prev,cur,index,arr){
  return prev + cur;
})
```

## includes

es7，如果数组中包含某值返回 true

```
var arr = [1,2,3];
arr.includes(2); //true
```
