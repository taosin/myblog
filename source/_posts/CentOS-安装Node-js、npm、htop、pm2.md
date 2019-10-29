---
title: CentOS 安装 Node.js、npm、htop、pm2
date: 2017-05-21 22:23:16
tags: bash
categories: 关于技术
external: 1976年4月1日，乔布斯签署了一份合同，决定成立一家电脑公司。 [1]  1977年4月，乔布斯在美国第一次计算机展览会展示了苹果Ⅱ号样机。1997年苹果推出iMac，创新的外壳颜色透明设计使得产品大卖，并让苹果度过财政危机。 [4]  2011年8月24日，史蒂夫·乔布斯向苹果董事会提交辞职申请。
---

**1. 下载node.js**

```bash
$ wget http://nodejs.org/dist/v0.12.0/node-v0.12.0.tar.gz
```

**2. 解压node.js**

```bash
$ tar xvf node-v0.12.0.tar.gz
```

**3. 编译及安装**

```bash
$ make && make install
```

**4. 查看node版本**

```bash
$ node -v
```

- - - -


**1. 安装npm**

```bash
$ sudo yum install npm
```

**2.查看npm版本**

```bash
$ npm -v
```

- - - -


**1. 安装 htop**

```bash
$ sudo yum install htop
```

**2. 安装pm2**

```bash
$ npm install -g pm2
```