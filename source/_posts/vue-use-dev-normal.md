---
title: vue.js组件使用和开发规范
date: 2017-07-13 09:28:38
tags: vue
categories: 关于技术
---
> 开发规范  

### 开发之前
#### 尽量使用最新ES语法，具体如下：

* 正确使用`const`和`let`替代`var`
* 使用模板字符串`${this.data}`
* 将工具函数等依赖单独分离，并用`import`导入
* 对象字面量缩写、箭头函数
* 通用工具集可以在`utils/assist`内扩展
* 在local/routers内测试组件

### 组件
#### 命名

* 尽量简单、表意
* `export` 出的对象是用驼峰命名法，比如`Page` 、 `ButtonItem`
* 如组件需要嵌套使用，子组件命名在父组件后加`-item`，比如`timeline`及`timeline-item`
#### 目录
* 组件在目录 `compoents/`下，每个组件单独一个目录，目录命名是用小写，入口文件为`index.js`，导出组件，再由`index.js` 暴露给使用者
* 每个组件的文件名使用小写， 但与组件的名称一致，比如 `timeline.vue`和`timeline-item.vue`
#### 属性
* 必须规定`type`或者使用`validator`进行验证
* 如果`validator`验证为几个值中的一个，则使用`utils/assist`内的`oneOf`函数进行验证
* 如果有尺寸的参数`size`，则只能使用`small` 、`large`，默认是适中，可不写

### 事件
#### 命名

* 使用`on-` 为前缀，比如`on-change`

#### 规范
* 使用`$emit`来对外触发事件，而不用`$dispatch`和`$broadcast`
* 嵌套组件之间通信，使用`$parent`和`$children`，而不用`$emit`，避免使用这错误使用自定义事件

### 其他
* css前缀

* 使用规范  
使用:prop 传递数据类型为`数字` 或 `boolean`时，必须带 `:`,比如：
```
正确的使用方法：
<Page :current="1" :total="100"></Page>
错误的使用方法：
<Page current="1" total="100"></Page>
```
