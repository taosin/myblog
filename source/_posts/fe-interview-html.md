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