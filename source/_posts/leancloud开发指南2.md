---
title: leancloud开发指南2
date: 2016-07-28 20:23:57
tags: leancloud
cateories: 技术整理
---

leadcloud数据存储开发指南(二)－Js篇
### 初始化
#### 首先注册并登录[leancloud](https://leancloud.cn),创建应用：


![屏幕快照 2016-07-28 14.49.18.png](http://upload-images.jianshu.io/upload_images/2222175-efa8ae9c117c602b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后打开 控制台 / 设置 / 应用 Key：
![145.pic.jpg](http://upload-images.jianshu.io/upload_images/2222175-8d3d1305a58a7350.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
前端项目使用leadcloud javascript sdk，在页面加载的时候调用初始化函数：
``` javascript
var APP_ID = 'apLrGX1xBjvsj3TROPuj41A6z2gasD-ZAFG';
var APP_KEY = 'pE0Y3fCPp01I1DBS4Nh4Gj2';
AV.init({
  appId: APP_ID,
  appKey: APP_KEY
});
```
 启用节点：
默认为大陆节点，如需切换到其他节点，可在初始化函数中加入：region: 节点，如下：
```
const APP_ID = 'apLrGX1xBjvsj3TROPuj41A6z2gasD-ZAFG';
const APP_KEY = 'pE0Y3fCPp01I1DBS4Nh4Gj2';
AV.init({
  appId,
  appKey,
  // 启用美国节点
  region: 'us',
});
```
### 验证
验证本地网络是否可以访问leadcloud服务器，
```
ping api.leancloud.cn
```
如果网络正常则得到如下相应：
```
PING api.leancloud.cn (120.132.49.239): 56 data bytes
64 bytes from 120.132.49.239: icmp_seq=3 ttl=49 time=65.165 ms
64 bytes from 120.132.49.239: icmp_seq=4 ttl=49 time=53.273 ms
64 bytes from 120.132.49.239: icmp_seq=5 ttl=49 time=51.519 ms
64 bytes from 120.132.49.239: icmp_seq=6 ttl=49 time=68.442 ms
```
然后在项目中编写测试代码：
```
const TestObject = AV.Object.extend('TestObject');
const testObject = new TestObject();
await testObject.save({ words: 'Hello World!' });
alert('LeanCloud Rocks!');
```
可在  控制台 > 存储 > 数据 > TestObject，中查看测试内容，至此，sdk安装完毕