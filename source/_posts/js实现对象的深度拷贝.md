---
title: js实现对象的深度拷贝
date: 2016-07-31 22:24:30
tags: javascript
categories: 关于技术
external: 复杂的数据类型即引用类型，它的值是对象，保存在堆内存中，包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针。从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终都指向同一个对象。
---

在前端页面中，有些时候数据的变化并没有驱动页面视图的变化，这个时候就需要深度拷贝，所以封装了一个使用原生JS深度的拷贝，代码如下：


``` Javascript
function deepCopy(oldObj) {
        // 定义一个新的空对象
        let newObject = {};
        // 判断原对象是否存在
        if(oldObj){
        // 判断原对象的类型
        if (oldObj.constructor === Object) {
            newObject = new oldObj.constructor();
        } else {
            newObject = new oldObj.constructor(oldObj.valueOf());
        }
        // 遍历克隆原对象的属性
        for (const key in oldObj) {
            if (newObject[key] !== oldObj[key]) {
                if (typeof(oldObj[key]) === 'object') {
                    // 对象内部的子对象
                    newObject[key] = deepCopy(oldObj[key]);
                } else {
                    newObject[key] = oldObj[key];
                }
            }
        }
        // 克隆原对象的常用方法
        newObject.toString = oldObj.toString;
        newObject.valueOf = oldObj.valueOf;
        return newObject;
        }
    }
```