---
title: NexT主题配置
date: 2016-07-18 23:10:48
tags: next
categories: 关于技术
photos: http://images.iamtaoxin.com/0e11af972a262b19c8f38df754c88348.png
---
NexT主题配置

-------
###添加「标签」页面
#### 新建页面
```
$ cd your-hexo-site
$ hexo new page tags
```
#### 设置页面类型
编辑之前心间的标签页面，内容如下：
```
title: 标签
date: 2014-12-22 12:39:04
type: "tags"
---
```
#### 修改菜单
编辑 主题配置文件 themes/_config.yml，内容如下：
```
menu:
  home: /
  archives: /archives
  tags: /tags
```
###添加「分类」页面
#### 设置步骤同 设置标签步骤
### 设置代码高亮主题
编辑 主题配置文件 themes/_config.yml，内容如下：
```
highlight_theme: normal
```
### 侧栏社交链接
编辑 主题配置文件 themes/_config.yml
#### 设置链接
```
# Social links
social:
 GitHub: https://github.com/taosin
  微博: http://www.weibo.com/p/1005055164097015/home?from=page_100505&mod=TAB&is_all=1#place
  # 等等
```
#### 设置图标
```
# Social Icons
social_icons:
  enable: true
  # Icon Mappings
  GitHub: github
  Twitter: twitter
  微博: weibo
```
### 设置打赏功能
编辑 主题配置文件 themes/_config.yml
```
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: http://7xs43y.com1.z0.glb.clouddn.com/u=1421062774,389829371&fm=21&gp=0.jpg
alipay: http://7xs43y.com1.z0.glb.clouddn.com/u=1421062774,389829371&fm=21&gp=0.jpg
```
### 设置友情链接
编辑 主题配置文件 themes/_config.yml
```
# title
links_title: Links
links:
  MacTalk: http://macshuo.com/
  Title: http://example.com/
```
### 腾讯公益404页面
在themes/source目录下新建404页面，内容如下：
```
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
</head>
<body>

<script type="text/javascript" src="http://www.qq.com/404/search_children.js"
        charset="utf-8" homePageUrl="your site url "
        homePageName="回到我的主页">
</script>

</body>
</html>
```
### 站点建立时间
编辑 主题配置文件 themes/_config.yml
```
since: 2013
```
-----
注：本文参考NexT官方文档，如需了解详细，请访问[NexT官网](http://theme-next.iissnan.com)。