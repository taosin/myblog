---
title: vue页面使用阿里oss上传图片功能(二)
date: 2017-05-24 14:35:08
tags: vue.js
categories: 关于技术
---

[源码](https://github.com/taosin/alioss-js-upload)


>主要介绍OSS管理控制台设置访问权限、角色等。

 进入控制台，鼠标移到右上角用户名处，点击“访问控制”,如下图：
![enter image description here](http://blog.xinsay.cn/oss_11.jpg)
如果没有此功能，则将鼠标移至 **产品**  -> **管理与控制**，点击 **访问控制**进入访问控制产品页
![enter image description here](http://blog.xinsay.cn/oss_123.png)

#### 1.创建子账号

* 点击左侧的 **用户管理** -> **自定义授权策略** ->**新建授权策略** ， 如下图：
![enter image description here](http://blog.xinsay.cn/oss_14.jpg)

#### 2.新建授权策略
* 点击左侧的 **策略管理** -> **新建用户** ， 如下图：

![enter image description here](http://blog.xinsay.cn/oss_15.jpg)

* 在弹出对话框中：选择授权策略模板（使用空模板）
* 编辑授权策略并提交：修改 授权策略名称（自定义名称），备注，策略内容，并提交。
**示例**：一个MNS授权策略内容模版:
```
{
						"Statement": [
					　　		{
								"Action": "mns:*",
								"Effect": "Allow",
								"Resource": "acs:mns:*:*:*" 
							}
						],
						"Version": "1"
				　 　}
```
**授权策略**是json格式的字符串，其中，
**Action** ： 表示要授权的操作，MNS 操作都以"mns:"开头，
例如: "mns:SendMessage" 表示 MNS 服务的API：SendMessage/BatchSenMessage
其他详见附录：MNS API和授权操作映射表；
**Effect** ： 表示授权类型，　例如:Allow, Deny
**Resrouce** : 表示要授权的阿里云资源名（ARN），格式为："acs:<云服务名>: <地域名>: <主账号UID>:<资源URI>"
例如：“acs:mns:cn-hangzhou:123456789:/queues/MyQueue1/messages”
表示：授权资源是主账号UID为123456789在cn-hangzhou地域的MyQueue1。
用"*"表示不指定具体的字段，例如："acs:mns:*:*:*" 表示不指定地域名,主账号ID和资源URI，子账号可以访问主账号的所有mns资源。

#### 3.授权子账号访问MNS
* 返回 **用户管理** ,找到第一步创建的子账号，点击右侧 **授权**
* 在弹出的对话框中，选择**授权策略名称**，并添加到右侧已选**授权策略**列表，点击 **确定** 提交，如下图：
* 
![enter image description here](http://blog.xinsay.cn/add_policy_to_user.jpg)

#### 4.创建角色

* 点击左侧的 **角色管理** -> **新建角色**如下图：

![enter image description here](http://blog.xinsay.cn/oss_13.jpg)
![enter image description here](http://blog.xinsay.cn/oss_16.jpg)


#### 4.授权策略访问角色
* 点击左侧的 **角色管理** ,在右侧的角色列表中选择需要授权的角色，点击**授权**，如下图：

![enter image description here](http://blog.xinsay.cn/oss_17.jpg)

#### 5.注意事项

1.server端的代码中 
```javascript
var sts = new STS({
	accessKeyId: '子账号 accessKeyId',
	accessKeySecret: '子账号 accessKeySecret',
});
```

accessKeyId 和 accessKeySecret 为第一步创建的子用户的 key

2. rolearn
```javascript
var rolearn = '对应角色的Arn值';
```
3.policy
```javascript
var policy = {
	"Version": "1",
	"Statement": [
	{
		"Effect": "Allow",
		"Action": [
		"oss:GetObject",
		"oss:PutObject"
		],
		"Resource": [
		"acs:oss:*:*:BucketName",
		"acs:oss:*:*:BucketName/*"
		]
	}
	]
};
```

这里的policy 必须和之前创建的策略一致。


