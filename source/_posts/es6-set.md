---
title: ES6之Set个人解读
date: 2018-02-07 17:05:11
tags: javascript
categories: 面试笔记
external: ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
---

参考链接：[ECMAScript 6 入门- Set 和 Map 数据结构](http://es6.ruanyifeng.com/#docs/set-map)

##### I. 定义：

Set 本身是一个构造函数，用来生成Set数据结构。

Set 是 ES6 中提供的新的数据结构，结构类似于数组，但 Set 中的成员的值都是唯一的，没有重复值。

Set 函数可以将一个数组作为参数，将其初始化。
```js
const arr = [1, 2, 3, 4, 4];
const set = new Set(arr);
// [1, 2, 3, 4, 4];
```

##### II. Set 实例的属性
* Set 实例有以下两个属性：

- `Set.prototype.constructor`：构造函数，默认就是 Set 函数。
- `Set.prototype.size`：返回 Set 实例的成员总数。


##### III. Set 实例的方法
* Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。四个操作方法如下：
 - `add(value)`: 添加某个值，返回 Set 结构本身。
 - `delete(value)`: 删除某个值，返回一个布尔值，表示删除是否成功。
 - `has(value)`: 返回一个布尔值，表示该值是否为Set的成员。
 - `clear(value)`: 清楚所有成员，没有返回值。

**初级用法：**

```js
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2); // true
s.has(2) // false
```

`Array.from` 方法可以将 Set 结构转为数组。
```js
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了数组去重的一种方式：
```js
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]
```

**中级用法**

遍历操作 

Set 实例的四个遍历方法：
* `keys()`: 返回键名的遍历器
* `values()`: 返回键值的遍历器
* `entries()`: 返回键值对的遍历器
* `forEach()`: 使用回调函数遍历每个成员

*需要特别指出的是，Set 的遍历顺序就是插入顺序。这个特性有时非常有用，比如在使用 Set 保存一个回调函数列表，调用时就能保证按照添加顺序调用。*

* (1) `keys()`, `values()`, `entries()`

	`keys`方法、`values`方法、`entries`方法返回的都是遍历器对象。由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。
```js
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

	上面代码中，`entries` 方法返回的遍历器，同事包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。
Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的 `values()` 方法。
```js
 Set.prototype[Symbol.iterator] === Set.prototype.values
// true
```

	这意味着，可以省略	`values` 方法，直接用	`for…of`遍历循环 Set。
```js
let set = new Set(['red', 'green', 'blue']);

for(let x of set){
	console.log(x);
}
// red
// green
// blue
```

* (2) `forEach()`

	`forEach` 方法用于对每个成员执行某种操作，没有返回值。
```js
set = new Set([1, 2, 4]);
set.forEach((value, key) => console.log(key + ':' + value))
// 1:1
// 2:2
// 4:4
```

	上面代码说明， `forEach` 方法的参数就是一个处理函数，该函数的参数与数组的 `forEach` 一致，依次为 键值、键名、集合本身 。Set 的键名和键值永远是一样的。
	另外，`forEach`方法还可以有第二个参数，表示绑定处理函数内部的 `this` 对象。

* (3)遍历的应用

	扩展运算符(`...`) 内部使用 `for…of` 循环，所以也可以用于 Set 结构。
```js
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```

	扩展运算符和 Set 结构相符合，就可以进行数组去重了。
```js
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```

**进阶方法**

数组的 `map` 和 `filter` 方法也可适用于 Set。

```js
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x* 2));
//  返回的set结构： {2, 4, 6};

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```

用 Set 实现并集（Union）、交集（Intersect）和差集（Difference）。

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用 `Array.from` 方法。

```js
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```

