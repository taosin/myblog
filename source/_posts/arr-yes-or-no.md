---
title: 判断一个对象是否为数组
date: 2018-05-17 17:57:07
tags: javascript
categories: 面试笔记
---

如何判断一个对象是不是 **Array**

    1. `Array.isArrya(obj)` 调用数组的 `isArray` 方法 ;

    2. `obj instanceOf Array` 判断对象是否是 `Array` 的示例 ;

    3. `Object.prototype.toString.call(obj) ===‘[object Array]’`

`Object.prototype.toString` 方法会取得对象的一个内部属性 `［［Class］］` ，然后依据这个属性，返回一个类似于`［object Array］`的字符串作为结果，`call` 用来改变 `toString` 的 `this` 指向为待检测的对象

    4. 判断对象是否有push等数组的一些方法。（这个方法有兼容问题，但也是一个简单易用的方法）

    5. `obj.constructor===Array   //true`

- **同理判断一个对象是否是函数**：

`console.log(Object.prototype.toString.call(obj)==='[object Function]') //true或false`
