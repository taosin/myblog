---
title: leadcloud数据存储开发指南(四)－Js篇
date: 2016-08-01 22:20:46
tags: leancloud
categories: 技术整理
external: 数据存储（LeanStorage）是 LeanCloud 提供的核心功能之一，它的使用方法与传统的关系型数据库有诸多不同。下面我们将其与传统数据库的使用方法进行对比，让大家有一个初步了解。
---
leadcloud数据存储开发指南(四)－Js篇
-------
保存对象
AV.Object 对象在保存时可以设置选项来快捷完成关联操作，可用的选项属性有：

| 选项 | 类型 | 说明 |
| ------------|:------------:|-------:|
| fetchWhenSave  | BOOL | 对象成功保存后，自动返回该对象在云端的最新数据。用途请参考 更新计数器 |
| query | AV.Query | 当 query 中的条件满足后对象才能成功保存，否则放弃保存，并返回错误码 305。|

示例：
```
new AV.Query('Wiki').first().then(function (data) {
    var wiki = data;
    var currentVersion = wiki.get('version');
    wiki.set('version', currentVersion + 1);
    wiki.save(null, {
      query: new AV.Query('Wiki').equalTo('version', currentVersion)
    }).then(function (data) {
    }, function (error) {
      if (error) {
        throw error;
      }
    });
  }, function (error) {
    if (error) {
      throw error;
    }
  });
```

获取对象
每个被成功保存在云端的对象会有一个唯一的 Id 标识 id，因此获取对象的最基本的方法就是根据 id 来查询：
```
  var query = new AV.Query('Todo');
  query.get('57328ca079bc44005c2472d0').then(function (data) {
    // 成功获得实例
    // data 就是 id 为 57328ca079bc44005c2472d0 的 Todo 对象实例
  }, function (error) {
    // 失败了
  });
```
如果不想使用查询，还可以通过从本地构建一个 id，然后调用接口从云端把这个 id 的数据拉取到本地，示例代码如下:
```
 // 第一个参数是 className，第二个参数是 objectId
  var todo = AV.Object.createWithoutData('Todo', '5745557f71cfe40068c6abe0');
  var title = todo.get('title');// 读取 title
  var content = todo.get('content');// 读取 content
```

获取 objectId
每一次对象存储成功之后，云端都会返回 id，它是一个全局唯一的属性。
```
 var todo = new Todo();
  todo.set('title', '工程师周会');
  todo.set('content', '每周工程师会议，周一下午2点');
  todo.save().then(function (todo) {
    // 成功保存之后，执行其他逻辑
    // 获取 objectId
    var objectId = todo.id;
  }, function (error) {
    // 失败之后执行其他逻辑
    console.log(error);
  });
```