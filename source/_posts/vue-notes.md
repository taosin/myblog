---
title: Vue常见知识点
date: 2018-03-10 11:41:58
tags: Vue.js
categories: 面试笔记
external: 不知不觉，使用 Vue 开发已经两年多了，从 1.0 到 2.0 ，Vue 伴随着我的整个前端开发历程，因此整理一些关于 Vue 的常见知识点。
---


##### I. 什么是 MVVM?

MVVM是 `Model-View-ViewModel` 的缩写。它是一种设计思想。`Model` 层代表数据模型，也可以在 `Model` 中定义数据修改和操作的业务逻辑；`View` 代表UI组件，它负责将数据模型转化成 UI展现出来；`ViewModel` 是一个同步 `View` 和 `Model` 的对象。
 
在**MVVM**架构下，`View` 和 `Model` 之间并没有直接的联系，而是通过 `ViewModel` 进行交互，`Model` 和 `ViewModel`之间的交互是双向的，因此 `View` 数据的变化会同步到 `Model` 中，而 `Model` 数据的变化也会立即反应到 `View` 上。
 
`ViewModel` 通过双向数据绑定把 `View` 层 和 `Mode` 层连接了起来，而 `View` 和 `Model` 之间的同步工作完全是自动的，不需要手动去操作 **DOM** 节点，不需要灌输主句状态的同步问题，复杂的数据状态维护完全由 **MVVM** 来统一管理。

##### II. MVVM 和 MVC 的区别？

**MVC** 和 **MVVM** 都是一种设计思想。主要区别是 **MVC** 中 `Controller` 演变成了 **MVVM** 中的 `ViewModel`。 **MVVM* 主要解决了 `MCV` 中大量的 **DOM** 操作是页面渲染性能降低，加载速度变慢，从而影响了用户体验。 而且当 `Model` 层数据频繁发生变化时，开发者需要手动更新 `View` 层。

##### III. Vue的优点是什么？
 
* a. 低耦合。视图层(View) 可以独立于 `Model` 变化和修改，一个 `ViewModel` 可以绑定到不同改的 `View` 上，当 `View` 发生变化 `Model` 可以不变，当 `Model` 变化的时候 `View` 也可以不变。

* b. 可重用性。你可以把一些视图层逻辑放在一个 `ViewModel` 中，让很多 `View` 重用这段视图逻辑。 

* c. 独立开发。开发人员可以专注于业务逻辑和数据的开发(`ViewModel`)，设计人员可以专注于页面设计，使用 `Expression Blend` 可以很容易设计界面并生成 **xml** 代码。

* d. 可测试。界面素来是比较难于测试的，而现在测试可以针对 `ViewModel` 来写专门的测试代码。

##### IV. Vue2.0生命周期是什么？

**Vue** 的生命周期总共有 创建前/后，载入前/后，销毁前/后。

* a. 创建前/后：在 `beforeCreate` 阶段，*Vue* 实例的挂载元素 `el` 和数据对象 `data` 都为 `undefined` ，还未初始化。在 `create` 阶段，*vue* 实例的数据对象  *data* 有了值，但 *el* 仍然没有。

* b. 载入前/后：在 `beforeMount` 阶段，*Vue* 实例的 `$el` 和 `data` 都初始化了，但还是挂载之前虚拟的 `DOM` 节点，`data.message` 还未替换。在 `mounted` 阶段，*Vue* 实例挂载完成，`data.message` 成功渲染。

* c. 更新前/后：当 `data` 发生变化时，会触发 `beforeUpdate` 和 `update` 方法。

* d. 销毁前/后: 在执行 `destroy` 方法后，对 `data`的改变不会再触发周期函数，说明此时 *Vue* 实例已经解除了事件监听以及和 `DOM` 的绑定，但是 `DOM`结构依然存在。

##### V. [Vue1.0](https://v1.vuejs.org/guide/) 和 [Vue2.0](https://vuejs.org/)的对比

* 1.片段代码: 在 `Vue2.0` 中，每个组件模板中，必须有一个根元素，来包裹所有的元素。
```html
<!-- 之前:   在1.0使用时完全没问题 -->
    <template>
        <h3>我是组件</h3><strong>我是加粗标签</strong>
    </template>
<!-- 现在:  必须有根元素，包裹住所有的代码 -->
    <template id="aaa">
            <div>
                <h3>我是组件</h3>
                <strong>我是加粗标签</strong>
            </div>
    </template>
```
* 2.生命周期见下表

|vue1.0| vue2.0| 描述|
|:-----|:------|:------|
| init | beforeCreate| 组件实例刚被创建，组件属性计算之前，如 data 属性等。|
| created | created | 组件实例创建完成，属性已绑定，但 **DOM** 还未生成，`$el`属性还不存在。|
| beforeCompile| boforeMount| 模板编译/挂载之前|
|compiled| mounted| 模板编辑/挂载之后|
| ready| mounted| 模板编译/挂载之后（不保证组件已在 `document` 中）|
|-|beforeUpdate|组件更新之前|
|-|updated|组件更新之后|
|-|activated| for `keep-alive` ，组件被激活时调用|
|-|deactivated| for `keep-alive` ，组件被移除时调用|
|attached| - |-|
|deattached|-|-|
|beforeDestroy| beforeDestroy| 组件销毁前调用|
|destroyed| destroyed| 组件销毁后调用|


* 3.过滤器
2.0 删除了 1.0 所有自带的过滤器，将不再是传参的方式调用，如下：
```js
{{msg | mimi '12' '5'}}
```

    而现在2.0中，要使用过滤器，必须要自定义一个过滤器:
```js
Vue.filter('toDou',function(n,a,b){
    return n<10?n+a+b:''+n;
});

//调用过滤器
{{msg | mimi('12','5')}}

```

* 4.`v-for`循环
    * 在1.0中循环渲染时会使用到 `tranck-by="$indec"`来提高for循环的性能，而在2.0，使用重复数据将不会报错，同时也去掉了一些隐式变量如：index 、 key，如果要用到 `index` 和 `key` 则可通过 ES6的语法来获取：
```html
    v-for="(val,index) in rows"
```
    * 关于整数循环，1.0的整数循环是从0开始的，2.0的整数循环则是从1开始的。
