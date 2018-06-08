---
title: 前端面试基础之JavaScript篇
date: 2018-05-20 17:27:00
tags: 前端面试
categories: 关于技术
---

### 1. 说说 JS 的数据类型

#### 1.1 基本类型

* String
* Number
* Boolean
* null
* undefined
* symbol (ES6中新增)

#### 1.2 引用类型

* Date
* RegExp
* Object
* Array
* Function ( 后三种为基本包装类型，特殊的引用类型，也算作基本类型)

#### 1.3 基本类型保存在栈上，引用类型的引用保存在栈上，其对象保存在堆中。

#### 1.4 `null` 和 `undefined` 的区别？

    - i. 含义不同：`null` 表示该处不该有值，而 `undefined` 表示该处有值，但还没有进行初始化
    - ii. 类型不同： `null` 的类型为 `Object`, `undefined` 的类型为 `undefined`

### 2. 变量提升

### 3. 对于闭包的理解，以及在什么场景下会使用到闭包？

#### 3.1 什么是闭包？
	
	闭包是指那些可以访问独立数据的函数。
	```js
		function init(){
			let name = 'Mozilla';
			// name 是一个被 init创建的局部变量
			function displayName(){
				// displayName() 是一个内部函数
				alert(name);
				// 一个闭包使用在父函数中声明的变量
			}
			displayName();
		}
	```

	这段代码中，`displayName` 是一个典型的闭包，它能够访问它之外的 `name` 变量，并保存这个变量在它创建时的状态。这个特性非常实用，因为 `Javascript` 有很多奇怪的坑

### 4. 谈谈对原型与原型链的了解度，有几种方式可以实现继承，用原型实现集成有什么缺点，要怎么解决？

#### 4.1 什么是原型链？

	原型链是 `Javascript` 实现继承最重要的一种方式。每一个对象都有自己的原型对象，根原型对象没有原型，所以其 `__proto__` 属性值为 `null` 。在调用时，如果访问的对象属性没有找到，`Javascript` 会顺着原型链继续往下找，直到触碰到根原型为止。

#### 4.2 原型与原型链的区别是什么？
	
`__proto__` 是隐式原型，指向创建这个对象的函数`(constructor)` 的 `prptotype`，用来构成原型链和实现基于原型的继承。

`protype` 是显式原型，用来实现基于原型的继承与属性的共享。每一个函数都具有 `prototype` 属性。

#### 4.3 在面向对象语言中，继承分为两种：

* 接口集成：只集成方法签名
* 实现集成：继承实际的方法

	因为 `Javascript` 没有继承的关键字，所以 JS 实现继承的方法很特殊，大概有以下几种：

	- 原型链继承(最常见)：

	```原型链继承
	var Base = function (){
		this.name = 'Base';
		this.toString = function (){
			return this.name;
		}
	}
	var Sub = function (){
		Base.call(this);
		this.name = 'Sub';
	}
	```

这种继承解决了原型链继承的问题，但它也有一些弊端：使用 `instanceof` 运算符的时候会发现，它的实例并不是父类的实例（因为父类并没有在它的原型链上）。

	- 实例继承

	```
	var Base = function (){
		this.name = 'Base';
		this.toString = function (){
			return this.name;
		}
	}
	var Sub = function(){
		var instance = new Base();
		instance.name = 'Sub';
		return instance;
	}
	```

这种继承方法只能强行被归类为继承，因为它实际上是返回了一个父类的实例，与子类毫无关系。同样，这种方法也不支持多继承。

	- 拷贝继承

	```
	var Base = function (){
		this.name = 'Base';
		this.toString = function (){
			return this.name;
		}
	}
	var Sub = function (){
		var base = new Base();
		for(var i in base){
			sub.prototype[i] = base[i];
		}
		sub.prototype['name'] = 'Sub';
	}
	```

首先，只有可枚举的对象类型才能使用 `foreach` 的方式获取到。它的优点是可以实现多继承，缺点是不方便写，效率低。

### 5. iframe 的缺点有哪些？

### 6. Ajax 的原生写法

### 7. 为什么会有同源策略

### 8. 前端解决跨域的几种方式

### 9. 怎么判断两个对象是否相等

### 10. 代码实现一个对象的深拷贝

### 11. 从发送一个url地址到返回页面内容，中间发生了什么？

### 12. 前端性能优化了解多少

	



