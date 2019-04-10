---
title: 使用 Node.js + Nginx 部署 HTTPS
date: 2018-06-08 16:13:33
tags: 服务器部署
categories: 关于技术
photos: http://images.iamtaoxin.com/0e11af972a262b19c8f38df754c88348.png
---

具体步骤：

1. 安装证书
```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx 

$ sudo certbot --nginx certonly
```


2. nginx 反向代理
在 `/ete/nginx/site-available/default` 中:
```bash
location /api {
	proxy_pass      http://localhost:8089;
	proxy_buffering on;
}
```

* 重启 `nginx`，重启 `node`。
