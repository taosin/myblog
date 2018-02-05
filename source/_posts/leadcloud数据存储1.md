---
title: leadcloud数据存储（一）-Javascript
date: 2016-07-27 22:06:34
tags: leancloud
categories: 关于技术
---
## 安装sdk
### 获取sdk
#### 包依赖管理工具安装
##### npm安装

```
# 存储服务
$ npm install leancloud-storage --save
# 实时消息服务
$ npm install leancloud-realtime --save
```

cnpm安装
```
$ npm install -g cnpm --registry=http://r.cnpmjs.org
```
然后执行
```
# 存储服务
$ cnpm install leancloud-storage --save
# 实时消息服务
$ cnpm install leancloud-realtime --save
```
##### bower安装
```
$ bower install leancloud-storage --save
```
##### cdn加速
```
<script src="https://cdn1.lncld.net/static/js/av-min-1.2.1.js"></script>
 ```
#### TypeScript 支持
##### 通过 typings 工具安装

首先需要安装 typings 命令行工具
```
npm install typings --global
```
然后执行如下命令：
```
typings install leadcloud-jssdk --save
```
######直接引用d.ts文件
typescript使用javascript SDK是通过定义文件夹来实现调用的，定义文件夹开源地址：[typed-leancloud-jssdk](https://github.com/leancloud/typed-leancloud-jssdk)

###### 手动安装

-----
参考地址[javascript数据存储开发指南](https://leancloud.cn/docs/leanstorage_guide-js.html)


