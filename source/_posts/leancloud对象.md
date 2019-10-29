---
title: leancloud对象
date: 2016-07-29 22:47:31
tags: leancloud
categories: 技术整理
external: LeanCloud 能够高效存取海量级 JSON 对象、二进制文件、地理位置等数据。其内置的行级 ACL 权限控制，以及通用的用户及角色管理体系，可以帮助您快速实现安全而灵活的数据访问。无服务器架构，让您快人一步。
---
leadcloud数据存储开发指南(三)－Js篇
----------
AV.Object 是 LeanStorage 对复杂对象的封装，每个 AV.Object 包含若干属性值对，也称键值对（key-value）。属性的值是与 JSON 格式兼容的数据。通过 REST API 保存对象需要将对象的数据通过 JSON 来编码。这个数据是无模式化的（Schema Free），这意味着你不需要提前标注每个对象上有哪些 key，你只需要随意设置 key-value 对就可以，云端会保存它。

数据类型：
AV.Object 支持以下类型：
```
 // 该语句应该只声明一次
  var TestObject = AV.Object.extend('DataTypeTest');

  var number = 2014;
  var string = 'famous film name is ' + number;
  var date = new Date();
  var array = [string, number];
  var object = { number: number, string: string };

  var testObject = new TestObject();
  testObject.set('testNumber', number);
  testObject.set('testString', string);
  testObject.set('testDate', date);
  testObject.set('testArray', array);
  testObject.set('testObject', object);
  testObject.set('testNull', null);
  testObject.save().then(function(testObject) {
    // 成功
  }, function(error) {
    // 失败
  });
```
注：每个 AV.Object 的大小都不应超过 128 KB。如果需要储存更多的数据，建议使用 AV.File。

构建对象：
构建一个AV.Object对象可以使用以下方式：
```
 // AV.Object.extend('className') 所需的参数 className 则表示对应的表名
  // 声明一个类型
  var Todo = AV.Object.extend('Todo');
```
注：每个 id 必须有一个 Class 类名称，这样云端才知道它的数据归属于哪张数据表。

保存对象：
现在我们保存一个 TodoFolder，它可以包含多个 Todo，类似于给行程按文件夹的方式分组。我们并不需要提前去后台创建这个名为 TodoFolder 的 Class 类，而仅需要执行如下代码，云端就会自动创建这个类：
```
  // 声明类型
  var TodoFolder = AV.Object.extend('TodoFolder');
  // 新建对象
  var todoFolder = new TodoFolder();
  // 设置名称
  todoFolder.set('name','工作');
  // 设置优先级
  todoFolder.set('priority',1);
  todoFolder.save().then(function (todo) {
    console.log('objectId is ' + todo.id);
  }, function (error) {
    console.log(error);
  });
```
创建完成后，打开 控制台 > 存储，点开 TodoFolder 类，就可以看到刚才添加的数据。除了 name、priority（优先级）之外，其他字段都是数据表的内置属性。

| 内置属性  | 类型	| 描述 |
| ------------|:------------:|-------:|
| id | String | 该对象唯一的Id标识 |
| ACL|	ACL | 该对象的权限控制，实际上是一个 JSON 对象，控制台做了展现优化。 |
| createdAt	| Date	| 该对象被创建的 UTC 时间，控制台做了针对当地时间的展现优化。   |
|updatedAt	| Date	| 该对象最后一次被修改的时间 |

属性名
也叫键或 key，必须是由字母、数字或下划线组成的字符串；自定义的属性名，不能以 __（双下划线）开头。
属性值
可以是字符串、数字、布尔值、数组或字典。

```  
以下为系统保留字段，不能作为属性名来使用。
acl             error            pendingKeys
ACL             fetchWhenSave    running
className       id               updatedAt
code            isDataReady      uuid
createdAt       keyValues
description     objectId
```
使用CQL语法保存对象
```
 // 执行 CQL 语句实现新增一个 TodoFolder 对象
  AV.Query.doCloudQuery('insert into TodoFolder(name, priority) values("工作", 1)').then(function (data) {
    // data 中的 results 是本次查询返回的结果，AV.Object 实例列表
    var results = data.results;
  }, function (error) {
    //查询失败，查看 error
    console.log(error);
  });
```
-----------
[leadcloud](https://leancloud.cn/docs/leanstorage_guide-js.html)
