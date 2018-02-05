---
title: vue页面使用阿里oss上传图片功能(一)
date: 2017-05-24 09:11:56
tags: vue.js
categories: 关于技术
---

[源码](https://github.com/taosin/alioss-js-upload)


直奔主题：

>## 前端部分

1.前端页面中需要引入oss-sdk:

```html
<script src="http://gosspublic.alicdn.com/aliyun-oss-sdk-4.4.4.min.js"></script>
```

2.自己封装的upload组件：

```html
<template>
	<div>
		<div class="oss_file">
			<input type="file" :id="id" :multiple="multiple" @change="doUpload"/>
		</div>
	</div>
</div>
</template>
```

```javascript
<script>
	export default{
		props:{
			multiple:{
				type: Boolean,
				twoWay:false
			},
			id:{
				type: String,
				twoWay:false
			},
			url:{
				type: Array,
				twoWay:true
			}
		},
		data(){
			return{
				region:'Your oss Region',
				bucket:'Bucket Name',
			};
		},
		methods:{
			doUpload(){
				const _this = this;
				Vue.http.get('/alioss/getOssToken').then((result) => {
					const client = new OSS.Wrapper({
						region:_this.region,
						accessKeyId: result.data.token.AccessKeyId,
						accessKeySecret: result.data.token.AccessKeySecret,
						stsToken: result.data.token.SecurityToken,
						bucket:_this.bucket
					})
					const files = document.getElementById(_this.id);
					if(files.files){
						const fileLen = document.getElementById(_this.id).files
						const resultUpload = [];		
						for (let i = 0; i < fileLen.length; i++) {
							const file = fileLen[i];
							const storeAs = file.name;
							client.multipartUpload(storeAs, file, {

							}).then((results) => {
								if(results.url){
									resultUpload.push(results.url);
								}
							}).catch((err) => {
								console.log(err);
							});
						}
						_this.url = resultUpload;
					}   
				});
			}
		}
	};
</script>
```

```css
<style type="text/css">
	.oss_file {
		height: 100px;
		width: 100%;

	}
	.oss_file  input {
		right: 0;
		top: 0;
		opacity: 0;
		filter: alpha(opacity=0);
		cursor: pointer;
		width: 100%;
		height: 100%;
	}

</style>
```

3.使用组件：

``` html
<template>
	<div>
		<div>
			<ali-upload :url.sync="uploadUrl" :multiple="true" :id="uploadId" :code.sync="uploadCode"></ali-upload>
		</div>
		<div v-for="url in uploadUrl">
			![](url)
		</div>
	</div>
</div>
</template>
<script>
	import aliUpload from './../components/aliossupload.vue';
	export default{
		components:{
			aliUpload
		},
		data(){
			return{
				uploadUrl:[],
				uploadId:'file',
				uploadCode:0
			};
		},
		watch:{
			uploadCode(val){
				console.info(val)
			}
		}
	};
</script>
```

>## 后端部分

1.首先安装依赖

```bash
npm install ali-oss
npm install co
```

2.service文件

```javascript
'use strict'
var OSS = require('ali-oss');
var STS = OSS.STS;
var co = require('co');

var sts = new STS({
	accessKeyId: 'accessKeyId',
	accessKeySecret: 'accessKeySecret',
});
var rolearn = 'acs:ram::ID:role/ram';

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
		"acs:oss:*:*:Bucket",
		"acs:oss:*:*:BucketName/*"
		]
	}
	]
};

class OssUploadService {

	getOssToken(req, res){
		var result = co(function* () {
			var token = yield sts.assumeRole(rolearn, policy, 15 * 60, 'ossupload');
			res.json({
				token:token.credentials
			})
		}).catch(function (err) {
		});
	}
}

module.exports = new OssUploadService()
```

2.controller文件

```javascript
'use strict'
var ossUploadService = require('../service/ossUploadService')
module.exports = function(app) {
  app.get('/alioss/getOssToken', function(req, res) {
    const result = ossUploadService.getOssToken(req, res)
    if (result) {
      res.json({
        code: 100,
        data: result
      })
    }
  })
}
```

>到这里就大功告成了吗？错！这只是完成了最基本的部分，接下来我们要在阿里云控制台中设置权限、角色、策略等。