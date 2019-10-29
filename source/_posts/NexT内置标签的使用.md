---
title: NexT内置标签的使用
date: 2016-07-23 17:46:26
tags: 博客搭建
categories: 技术整理
---
NexT内置标签的使用

### 文本居中
#### 使用方法
##### HTML方式:
```
<blockquote class="blockquote-center">这个城市的灯火辉煌，与你无关</blockquote>
```
##### 标签方式：
```
<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% centerquote %} 这个城市的灯火辉煌，与你无关 {% endcenterquote %}
<!-- 别名 -->
{% cq %} 这个城市的灯火辉煌，与你无关 {% endcq %}
```
示例：
<img src="http://7xs43y.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2018.06.09.png"/>
文字解释：此标签将生成一个带上下分割线的引用，同时引用内文本将自动居中。

### 图片突破容器宽度限制
#### 使用方式
#### HTML方式：
```
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="full-image" 是必须的 -->
<img src="/image-url" class="full-image" />
```
#### 标签方式：
```
<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% fullimage /image-url, alt, title %}

<!-- 别名 -->
{% fi /image-url, alt, title %}
```
示例：
<img src="http://7xs43y.com1.z0.glb.clouddn.com/%E5%A4%96%E6%BB%A9.jpg" class="full-image" />
文字解释：当使用此标签引用图片时，图片将自动扩大 26%，并突破文章容器的宽度。 此标签使用于需要突出显示的图片, 图片的扩大与容器的偏差从视觉上提升图片的吸引力。