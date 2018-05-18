---
title: 使用axios发送POST请求时后端获取不到参数的问题
date: 2018-05-18 17:21:39
tags: 前端
categories: 关于技术
---

在使用 `vue2` + `axios` + `node.js` + `express` 开发管理后台时，遇到了*axios发送POST请求时后端获取不到参数的问题* ，查阅资料许久，还是未找到解决办法，无奈之下，喝一口凉水，歇息一番，继续寻求良方。

终于，皇天不负有心人，[在这里](https://zhuanlan.zhihu.com/p/27498996)，还是找到了解决的办法。



``` js
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

由此看出，`axios` 在 `POST` 提交数据时也是使用的这种方式，在 `java` 和 `php` 可以获取到对应的 `request` 原始流数据获取到对应的值并将其序列化.


但是这样的方式在 `express` 服务后端并不能获取到数据，因此 `express` 中使用的是 `body-parser` 去格式化前台传来的数据，具体实现看代码：


```js
//server

'use strict'
var express = require('express')
var powerexpress = require('power-express')(express)
var authority = require('./filter/authority')
var app = powerexpress()

// 使用 body-parse 格式化数据
var bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({
    extended: true
}))
app.use(bodyParser.json())



var Server = require('./server')
var cookieParser = require('cookie-parser')
app.use(cookieParser())
require('./controllers/routes')(app)
var appServer = new Server(app)
appServer.start()
