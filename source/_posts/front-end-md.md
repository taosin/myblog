---
title: 前端面试知识点
date: 2018-02-09 14:57:44
tags: html
categories: 面试笔记
---



##### 1. CSS盒模型
* css属性：分为W3C标准盒模型和IE标准盒模型。大多数浏览器采用W3C标准模型，而IE中采用微软自己的标准。
* 怪异盒模型：是指 部分浏览器在采用W3C标准的同时还保留了原来的解析模式，这种情况主要表现在IE内核的浏览器中。
* 怪异模式的触发：当不对 `Doctype` 进行定义时，则会触发怪异模式。

* 标准模式下：一个块的总宽度 = `width + margin(左右) + padding(左右) + border(左右)` 。
* 怪异模式下：一个块的总宽度 = `width + margin(左右)` ，因为 `width` 已经包含了 `padding` 和 `border` 值。

#### 2. box-sizing 应用场景
* 语法：box-sizing:content-box || border-box || inherit;
* 使用场景： 
1.特殊场景的布局：假设一个场景，设置子类元素的 `margin` 或者 `border` 时，可能会撑破父层元素的尺寸，这时我就需要使用 `box-sizing: border-box` 来讲 `border` 包含进元素的尺寸中，这样就不会存在撑破父层元素的情况了。
2.统一风格的表格元素：表单中有些input展现的还是传统的IE盒模型，带有一些默认的样式，而且在不同平台或者浏览器下的表现不一，造成了表单层展现的差异，此时就可以通过 `box-sizing` 属性来构建一个风格统一的表单元素。

#### 3.Flex布局。
* 09年W3C提出了Flex布局，可以简便、完整、响应地实现各种页面布局。
* Flex是Flexible Box的缩写，意味“弹性布局”，用来为盒装模型提供最大的灵活性。
* 网页布局的传统解决方案，基于盒状模型，依赖 `display` 属性 + `position` 属性 + `float`属性。它对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。
* 用法

```css
# 任何一个容器都可以使用Flex布局
.box{
  display: flex;
}

# 行内元素也可以使用Flex布局
.box{
  display: inline-flex;
}

# Webkit 内核的浏览器，必须加上 -webkit 前缀
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

*注意，设为 Flex 布局以后，子元素的 `float` 、`clear` 和`vertical-align` 属性将失效。*

* 采用Flex布局的元素，都成为 **Flex容器**，所有子元素自动成为容器成员，成为 **Flex项目**。
* 水平的主轴（main axis）和垂直的交叉轴（cross axis）。
* 主轴的开始位置（与边框的交叉点）叫做 `main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。

* 属性：
1. flex-direction: 决定主轴的方向，即排列方向。
```css
.box {
  flex-direction: *row →| row-reverse ←| column ↓| column-reverse ↑;
}
```
2. flex-wrap: 决定一条轴线上项目的换行。
```css
.box{
  flex-wrap: *nowrap 不换行| wrap 换行第一行在上方| wrap-reverse  换行第一行在下方;
}
```
3. flex-flow: flex-direction  和 flex-wrap 的简写。 默认值为 `row nowrap`。
```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```
4. justify-content: 定义项目在主轴上的对齐方式。
```css
.box {
  justify-content: *flex-start 左对齐| flex-end 右对齐| center 居中| space-between 两端对齐| space-around 每个项目两侧的间隔相等;
}
```
5. align-items: 定义项目在交叉轴上的对齐方式。
```css
.box {
  align-items: flex-start 起点对齐| flex-end 终点对齐| center 中点对齐| baseline 基线对齐| stretch 占满整个容器的高度;
}

```
6. aligin-content: 定义多根轴线的对齐方式。
```css
box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

* 项目属性
1. order: 定义项目的排列属性。数值越小，排列越靠前，默认为0.
```css
.item{
  order: <integer>;
}
```
2. flex-grow: 定义项目的放大比例，默认为0，表如果存在剩余空间，也不能放大。
```css
item {
  flex-grow: <number>; /* default 0 */
}
```
3. flex-shrink: 定义项目的缩小比例，默认为1，表如果空间不足，该项目将缩小。
```css
.item {
  flex-shrink: <number>; /* default 1 */    0则不缩小
}
```
4. flex-basis: 定义在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性进行计算主轴是否有多余空间。
```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```
5. flex: 是 `flex-grow` , `flex-shrink` 和 `flex-basis` 的简写， 默认值为 `0 1 auto`。
```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

6. align-self: 允许单个项目有与其他项目不一样的对齐方式，可覆盖 `align-items` 属性。默认值为`auto`，表示继承父元素的`align-items` 属性，如果没有父元素，则等同于`stretch`。
```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

#### 4.未知元素上下左右垂直居中
* flex实现方式：

```css
.div{
  justify-content: center;
  align-items: center;
}
```

#### 5.js对象、原型链、构造函数

*  `__proto__`:每个JS对象一定对应一个原型对象，并继承原型对象的属性和方法。
* prototye: 只有函数才有 `prototype`属性。JS通过函数来模拟类，当创建函数时，JS会为这个函数自动添加`prototype`属性，值是空对象。而一旦把这个函数当做构造函数（`constructor`）调用（即通过`new`调用）,JS就会帮你创建该构造函数的实例，实例继承构造函数`prototype`的所有属性和方法（实例通过设置自己的 `__proto__` 指向承构造函数的prototype来实现这种继承）。
* JS通过`__proto__` 和`prototype`的合作实现了原型链，以及对象的继承。
* 构造函数，通过 `protytype`来存储要共享的属性和方法，也可以设置`prototype`指向现存的对象来继承该对象。

```js
var one = {x: 1};
var two = new Object();
one.__proto__ === Object.prototype // true
two.__proto__ === Object.prototype // true
one.toString === one.__proto__.toString // true
```
**对象的 `__proto__` 指向自己的构造函数的 `prototype` 。`one.__proto__.__proto__...` 的原型链有次产生，包括操作符 `instanceof` 正是通过探测 `obj.__proto__.__proto__... === Constructor.prototype` 来验证 `obj` 是否是 `Constructor` 的实例。
回到开头的代码，`two = new Object()`中Object是构造函数，所以 `two.__proto__` 就是 `Object.prototype` 。至于 `one` ，ES规范定义对象字面量的原型就是 `Object.prototype` 。**


* `Function.prototype` 和 `Function.__proto__` 都指向 `Function.prototype` 。
* `Object.prototype.__proto__ == null` ,说明原型链到 `Object.prototye` 终止。

结论：

* 1. 所有对象均从 `Object.prototye` 继承属性。
* 2. `Function.prototype` 和 `Function.prototype` 为同一个对象。
* 3. `Function.prototype` 直接继承 `root(Object.prototype)` 。

**一句话总结：先有`Object.prototype`(原型链顶端),`Function.prototype`继承`Object.prototype`而产生，`Function`和`Object`和其他构造函数继承`Function.prototype`而产生。**


#### 6.常见的布局解决方案
* 1.居中布局
    -  水平居中
    -  垂直居中

* 2.单列布局：
    - 定宽，水平居中
* 3.二列布局&三列布局
  - `float + margin` 左右浮动，中间 `margin`
  - position + margin 左右绝对定位，中间 `margin`
  - 圣杯布局 `(float + 负margin + padding + position)`
  - 双飞翼布局 `(float + 负margin + margin)`
  - `Flex` 布局


#### 7.关于WebSocket

* WebSocket 是 HTML5 提出的一个协议规范。
* WebSocket 约定了一个通信的规范，通过一个握手的机制，客户端（浏览器）和服务器（webserver）之间能简历一个类型TCP 的链接，从而方便 c-s 之间的通信。在 websocket 出现之前，web 交互一般是基于 http 协议的短连接或者长连接。
* WebSocket 是为解决客户端与服务端实时通信而产生的技术。websocket 协议本质上是一个基于TCP的协议，是先通过 HTTP/HTTPS 协议发起一条特殊的 http 请求进行握手后创建一个用于交换的TCP连接，此后服务端与客户端通过此TCP连接进行实时通信。
* **优点**： 服务器和客户端可以在给定的时间范围内的任意时刻，相互推送信息。浏览器和服务器只需要做一个握手的动作，在建立连接之后，服务器可以主动传送数据给客户端，客户端也可以随时向服务器发送数据。此外，服务器与客户端之间叫唤的标头信息很小。
* 以前 web server 实现推送技术或者即时通讯，用的都是轮询（polling），在特定的时间间隔由浏览器自动发出请求，将服务器的消息主动拉取，在这种情况下，浏览器需要不断地向服务器发送请求，然而 HTTP request 的header 是非常长的，里面包含的数据可能只是一个很小的值，这样就会占用很多的带宽和服务器资源。

















