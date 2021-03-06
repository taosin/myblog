---
title: vue+leancloud轻松打造个人博客
date: 2017-05-20 21:50:44
tags: 博客搭建
categories: 技术整理
---

基于 Vue.js 的纯前端博客

- 通过 Vue 构建博客前端框架
- 前端代码自动化部署到阿里云 cdn
- 使用阿里云和 leancloud 构建一个强大的个人博客

---

项目采用前端使用 vue 1.0 做为开发框架，leancloud 为本站提供数据储存服务。所有静态文件存放在阿里 OSS 上，无需另外购买服务器，这对于个人来说省了不少费用。

博客说明：该项目是个人博客 [iamtaoxin](http://blog.iamtaoxin.com) 的前端源码，它属于博客系统的一部分，博客系统一共分 3 部分:

1. 基于 Vue 1.0 构建的 Vuex 架构的博客前端页面
2. 使用 [`Leancloud`](http://leancloud.cn) 作为数据存储
3. 收藏阅读笔记的 Safari 插件

源码地址： [Github](https://github.com/taosin/ixinyi_admin)

## 具体步骤如下：

**一、获取代码，安装依赖**
拉取博客代码至本地目录文件夹，进入文件加中 public 中安装依赖程序。

```bash
$ npm install
```

**二、安装 leancloud SDK**
2.1 在`index.html`中添加以下代码：

<!-- 存储服务 -->
<script src="//cdn1.lncld.net/static/js/2.1.4/av-min.js"></script>

**_具体安装指南见 ：_**
JavaScript SDK 安装指南

2.2leancloud 初始化
在`ext/vue_ext.js`文件中已封装好`leancloud`初始化的方法，如下：

```javascript
function AVInit() {
  const appId = "Your appID";
  const appKey = "Your appkey";
  AV.init({ appId: appId, appKey: appKey });
}
```

只需在 App.vue 中调用即可:

```javascript
ready(){
    this.$AVInit();
}
```

**三、启动项目**

```bash
$ npm run dev
```

**四、部署项目**

部署项目中使用到了`aliyunoss-webpack-plugin`

```bash
$ npm install aliyunoss-webpack-plugin --save-dev
```

在 `webpack.prod.conf.js` 中配置自己的路径和相应的 key

```javascript
new AliyunossWebpackPlugin({
  buildPath: __dirname + "/build",
  region: "your region",
  accessKeyId: "your accessKeyId",
  accessKeySecret: "your accessKeySecret",
  bucket: "your bucket",
  deleteAll: true,
  getObjectHeaders: function(filename) {
    return {
      "Cache-Control": "max-age=2592000"
    };
  }
});
```

**五、部署项目**

```bash
$ npm run build
```

至此，项目部署已完成，配置相应的域名即可访问。

---

书写完毕，如有欠缺，请多指教。
