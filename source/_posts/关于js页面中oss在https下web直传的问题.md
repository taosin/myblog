---
title: 关于js页面中oss在https下web直传的问题
date: 2017-05-24 16:19:44
tags: javascript
categories: 关于技术
---

在页面中进行 `web端` 直传，开发环境下测试没有问题，但部署到服务器上时出现了问题，服务器上的站点是基于 `https` 协议进行访问的，在此情况下，`web端` 直传出现了异常，原因是在 `HTTPS` 网页中，不允许发起 `HTTP` 的请求，可以使用 `https` 的 `endpoint` ，示例如下：

```js
var client = new OSS.Wrapper({ 
  region: 'oss-cn-shanghai', 
  secure: true,     
  // 为true时，即允许发起http请求
  accessKeyId: '', 
  accessKeySecret: '' 
}); 
```