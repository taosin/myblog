---
title: 前端JS面试必须知道的几个点
date: 2018-05-17 17:54:44
tags: javascript
categories: 面试笔记
external: 总结了一些在面试过程中会遇到的前端问题，本文章主要是Javascript部分。
---


##### I.函数的三种定义方法
###### 1.1 函数声明

```js
// ES5
function getSum()
function (){} //匿名函数

// ES6
() => {} //如果 {} 内容只有一行 {} 和return 关键字可省略
```
###### 1.2 函数表达式（函数字面量）

```js
// ES5
var sum = function getSum()

// ES6
let sum = () => {} //如果{} 内容只有一行 {} 和return 关键字可省略
```

###### 1.3 构造函数
```js
var sum =  new GetSum(num1, num2)
```

###### 1.4 三种方法的比较

1. 函数声明有预解析，而且函数声明的优先级高于变量；
2. 使用 `Function` 构造函数定义函数的人方式是一个函数表达式，这种方式会导致解析两次代码，影响性能。第一次解析常规的 `Javascript` 代码，第二次解析传入构造函数的字符串。

##### II. ES5 中函数的 4种调用
###### 2.1 函数调用模式

该模式包括 函数名() 和匿名函数调用， `this` 指向 `window`

```js
function getSum(){
   console.log(this)   //window 
}

getSum()

(function(){
    console.log(this)  //window
})()


var getSum =  function(){
    console.log(this)  //window
}

getSum()
```

###### 2.2 方法调用

对象.方法名(), `this` 指向 对象
```js
var objList = {
    name: 'methods',
    getSum: function(){
        console.log(this)  //objList对象
    }
}
objList.getSum()
```

###### 2.3 构造器调用

`new` 构造函数名 (), `this` 指向构造函数

```js
function Person(){
    console.log(this);   //指向构造函数Person
}
var personOne = new Person();
```

###### 2.4 间接调用

利用 `call` 和 `apply` 来实现，`this` 就是 `call` 和 `apply` 对应的第一个参数，如果不传值或者第一个值为 `null` , `undefined` 时 指向 `window`

```js
function foo(){
    console.log(this);
}

foo.apply('我是apply改变的this值');
foo.call('我是call改变的this值');
```

##### III. ES6中函数的调用

箭头函数不可以当作构造函数，也就是不能用 `new` 命令实例化一个对象，否则会抛出一个错误
箭头函数的 `this` 是和定义时有关，和调用无关
调用就是函数调用模式

```js
(()=>{
console.log(this); //window
})

let arrowFun = () =>{
    console.log(this) //window
}

arrowFun()

let arrowObj = {
    arrFun: function (){
        (() => {
            console.log(this) //arrowObj
        })()
        
    }
}
arrowObj.arrFun
```

##### IV. `call`, `apply` 和 `bind`

* IE5之前不支持 `call` 和 `apply`，`bind` 是 **ES5** 之后出来的；
* `call` 和 `apply` 可以调用函数，改变 `this`,实现继承和借用别的对象的方法。

###### 4.1 `call` 和 `apply` 定义

调用方法，用一个对象替换掉另一个对象 (this)
对象.call(新 this 对象，实参1，实参2，实参3...)
对象.apply(新 this 对象，[实参1，实参2，实参3...])

###### 4.2 `call` 和`apply` 用法

* a. 间接调用函数，改变作用域的 `this` 值；
* b. 劫持其他对象的方法
```js
var foo = {
    name: '张三',
    logName:function(){
        console.log(this.name);
    }
}

var bar = {
    name: '李四'
};
foo.logName.call(bar); //李四
// 实质是call改变了foo的this指向为bar，并调用函数
```
* c. 两个函数实现继承
```js
<div style="font-size:20px" >
function Animal(name){
   this.name = name;
   this.showName = function (){
       console.log(this.name);
   }
}
function Cat(name){
    Animal.call(this, name);
}
var cat = new Cat('Black Cat');
cat.showName(); // Black Cat
</div>
```

* d. 为类数组(`arguments` 和`nodeList`)添加数组方法 `push`，`pop`
```js
(function (){
    Array.prototype.push.call(arguments,'王五');
    console.log(arguments); //['张三','李四','王五']
})('张三','李四');
```

* e. 合并数组
```js
let arr1 = [1,2,3];
let arr2 = [4,5,6];
Array.prototype.push.apply(arr1,arr2); //将arr2合并到了arr1中
```
* f. 求数组最大值
```js
Math.max.apply(null,arr)
```

* g. 判断字符类型
```js
Object.prototype.toString.call({})
// [object Object]
```

##### V. JS常见的四种设计模式

###### 5.1 工厂模式

简单的工厂模式可以理解为解决多个相似的问题；
```js
function CreatePerson(name,age,sex){
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function (){
        return this.name;
    }
    return obj;
}

var p1 = new CreatePerson('Tom','28','男');
var p2 = new CreatePerson('Lisa','21','女');

console.log(p1.name); // longen
console.log(p1.age);  // 28
console.log(p1.sex);  // 男
console.log(p1.sayName()); // longen

console.log(p2.name);  // tugenhua
console.log(p2.age);   // 27
console.log(p2.sex);   // 女
console.log(p2.sayName()); // tugenhua
```
###### 5.2 单例模式

只能被实例化(构造函数给实例添加属性与方法)一次
```js
//单体模式
var Singleton = function (name){
    this.name = name;
};

Singleton.prototype.getName = function(){
    return this.name;
}

//获取实例对象
var getInstance = (function (){
    var instance = null;
    return function (name){
        if(!instance){ //相当于一个一次性阀门，只能实例化一次
            instance = new Singleton(name);
        }
        return instance;
    }
})();

//测试单体模式的实例，所以 a===b
var a = getInstance('aa');
var a = getInstance('bb');
```

###### 5.3 沙箱模式
将一些函数放到自执行函数里面，但要闭包暴露接口，用变量接受暴露的接口，再调用里面的值，否则无法使用里面的值。
```js
let sanboxModel = (function(){
    function sayName(){};
    function sayAge(){};
    return {
        sayName: sayName,
        sayAge: sayAge
    }
})()
```

###### 5.4 发布者订阅模式

就例如我们关注了某一个公众号，然后他对应的有新的消息就会给你推送。
```js
//发布者与订阅模式

var shoeObj = {}; // 定义发布者
shoeObj.list = []; //缓存列表 存放订阅者回调函数

// 增加订阅者

shoeObj.listen = function(fn){
    shoeObj.list.push(fn);  // 订阅消息添加到缓存列表
}

// 发布消息
shoeObj.trigger = function(){
    for(var i = 0, fn; fn = this.list[i++];){
        fn.apply(this, arguments); //第二个参数只是改变fn的this
    }
}

// 小红订阅如下消息
shoeObj.listen(function(color,size){
    console.log('颜色是: '+ color);
    console.log('尺码是：' + size);
});

// 小花订阅如下消息
shoeObj.listen(function(color,size){
    console.log('再次打印颜色是: '+ color);
    console.log('再次打印尺码是：' + size);
});

shoeObj.trigger("红色", 40);
shoeObj.trigger("黑色", 42); 
```
代码实现逻辑是用数组存储订阅者，发布者毁掉函数里面通知的方式是遍历订阅者数组，并将发布者内容传入订阅者数组

##### VI. 原型链

###### 6.1 定义
对象继承属性的一个链条

###### 6.2 构造函数，实例与原型对象的关系

![image](https://segmentfault.com/img/bV8wcf?w=638&h=241/view)

```js
var person = function (name) {this.name = name}; //person是构造函数

var o3personTwo = new person('personTwo'); //personTwo是实例
```
![iamge](https://segmentfault.com/img/bV8wdm?w=534&h=333)

原型对象都有一个默认的`constructor`属性指向构造函数

###### 6.3 创建实例的方法

* a. 字面量
```js
let obj = {'name':'张三'}
```
* b. `Object` 构造函数创建
```js
let Obj = new Object();
Obj.name = '张三';
```

* c. 使用工厂模式创建对象
```js
function createPerson(name){
    var o = new Object();
    o.name = name;
    return o;
};

var person1 = createPerson('张三');
```

* d. 使用构造函数创建对象
```js
function Person(name){
    this.name = name;
}
var person1 = new Person('张三')；
```

###### 6.4 `new` 运算符

* a. 创建了一个新对象；
* b. `this`指向构造函数；
* c. 构造函数有返回，会替换 `new` 出来的对象，如果没有就是 `new` 出来的对象
* d. 手动封装一个 `new` 运算符
```js
var new2 = function (func){
        var o = Object.create(func.protorype);   //创建对象
        var k = func.call(o);    //改变this指向，把结果赋给k
        if(typeof k === 'object'){   //判断k的类型是不是对象
            return k;
        } else {
            return o;
        }
    }
```

###### 6.5 对象的原型链

![image](https://segmentfault.com/img/bV8wf4?w=570&h=709)

##### VII. 继承的方式

JS是一门弱类型动态语言，封装和继承是他的两大特性：

###### 7.1 原型链继承