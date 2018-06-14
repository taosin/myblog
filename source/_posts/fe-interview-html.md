---
title: 前端面试之HTML篇
date: 2018-06-13 07:49:47
tags: 前端面试
categories: 关于技术
---

### 1. CSS 三列布局常用的实现方案（左右定宽，中间自适应）

* 圣杯布局 Holy Grail Layout
	
原理：利用 **CSS** 负边距(Negative Margin)强行将左右两边的 DIV  移动到 中间  DIV  的左右。

Ex:

```html
<body>
	<div class="container">
		<div class="middle"></div>
		<!-- 先渲染middle -->
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
```

相应的 style  如下 ：

```css
.container{
	padding: 0 200px;
}

.container > div {
	height: 200px;
	float: left;
}
.middle{
	width: 100%;
	background: blue;
}

.left{
	margin-left: -100%;
	position: relative;
	left: -200px;
	width: 200px;
	background: red;
}
.right {
	margin-left: -200px; /* 用 -200px 吃掉 .right 自己的宽度 */
	position: relative; /* 相对 .container -200px */
	right: -200px;
	width: 200px;
	background: green;
}
```

* 双飞翼布局 

```html
<body>
    <div class="container">
        <div class="middle">
            <div class="middle-container"></div>
        </div>
        <div class="left"></div>
        <div class="right"></div>
    </div>
</body>
```

```css
.container > div {
    float: left;
    height: 200px;
}
.middle {
    width: 100%;
    background: red;
}
.middle-container { /* 添加 .middle-container 包裹内容 */
    height: 200px;
    margin: 0 200px;
    background: darkred;
}
.left {
    width: 200px;
    margin-left: -100%;
    background: blue;
}
.right {
    width: 200px;
    margin-left: -200px;
    background: green;
}
```

* Flex 布局 

```html
<body>
    <div class="container">
        <div class="col-1"></div>
        <div class="col-2"></div>
        <div class="col-3"></div>
    </div>
</body>
```

```css
.container {
    display: flex;
    width: 100%;
}
.container > div {
    height: 100px;
    border: 1px solid #000;
    flex: 1;
    position: relative;
}
```

### CSS 如何实现垂直居中？

* 父标签上指定高度 (height) 与行度 (line-height)相同即可：

```html
<body>
    <div class="vertical"></div>
</body>
```

```css
html, body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
}
.vertical {
    width: 100px;
    height: 100px;
    background: red;
    margin: 0 auto; /* 最简单的水平居中方式 */
    /* 以下是垂直居中的代码： */
    position: relative;
    top: 50%;
    margin-top: -50px;
}
```

* 使用 **Flex Layout** 中的 `align-items` 和 `justify-content` 实现居中

```html
<body>
    <div class="vertical"></div>
</body>
```

```css
html {
    height: 100%;
    width: 100%;
}
body {
    /* 使用 CSS3 flex layout 使元素垂直居中 */
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    padding: 0;
    height: 100%;
}
.vertical {
    width: 100px;
    height: 100px;
    background: red;
}
```

> <span style="color:orange">注：但是这种方法存在兼容性问题，下图是各个浏览器的支持情况：</span>

![flex 在各浏览器的支持情况](http://images.iamtaoxin.com/20160922210759487.png)

> Flex 教程可参考 [Flex 布局教程:语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)