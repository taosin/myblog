---
title: hexo使用NexT主题
date: 2016-07-17 18:52:28
tags: next
categories: 关于技术
---
## hexo与NexT主题
## 关于NexT
NexT 主旨在于简洁优雅且易于使用，所以首先要尽量确保 NexT 的简洁易用性，github地址：https://github.com/iissnan/hexo-theme-next。
### 使用NExT
#### 下载主题
进入到目标目录，使用git checkout代码：
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
#### 启用主题
打开配置文件_config.yml
修改theme字段
```
theme:next
```
#### 验证主题

```
hexo s --debug
```
访问浏览器， http://localhost:4000

#### 主题设定
Next共有三种外观： Muse, Mist , Piscec,
在tmemes\next\_config.yml中进行更改
```
#scheme: Muse
#scheme: Mist
scheme: Pisces
```
####  设置语言
编辑站点配置文件，设置所需语言，
|语言 |	代码 |	设定示例
|English |	en |	language: en
简体中文 	|zh-Hans 	|language: zh-Hans
繁體中文 	|zh-hk 或者 zh-tw | 	language: zh-hk

####  设置菜单
```
menu:
  home: /
  archives: /archives
  #about: /about
  #categories: /categories
  tags: /tags
  #commonweal: /404.html
  ```

####  设置头像
编辑 站点配置文件， 新增字段 avatar， 值设置成头像的链接地址  
eg: avatar: http://example.com/avtar.png