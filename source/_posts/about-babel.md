---
title: Babel 入门教程
date: 2019-07-08
tags: Babel
category: 技术整理
vssue-title: about-babel
---

作用：将 ES6+ 版本的代码转换为 ES5代码，以便能够运行在当前环境和旧版浏览器环境中。

![Babel](http://images.iamtaoxin.com/bg2016012501.png)

这就意味着，可以用 ES6 编写代码，而不用担心当前环境是否支持的问题。
<!-- more -->
## 一、配置 .babelrc

使用 Babel的第一步，就是配置 `.babelrc` 这个文件，该文件存放在项目根目录下，用来设置转码规则和插件，基本格式如下：

```js
{
	"presets": [...],
	"plugins": [...]
}
```

其中 `presets` 字段设定转码规则，可根据需要安装。

```js
# ES2015转码规则
$ npm install —save-dev babel-preset-es2015

# react转码规则
$ npm install —save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install —save-dev babel-preset-stage-0
$ npm install —save-dev babel-preset-stage-1
$ npm install —save-dev babel-preset-stage-2
$ npm install —save-dev babel-preset-stage-3
```

然后将这些规则加入 `.babelrc`:

```js
	{
		"presets": [
			"es2015",
			"react",
			"stage-2"
		],
		"plugins": [...]
}
```

## 二、命令行转码 babel-cli

Babel 提供 `babel-cli` 工具，用于命令行转码。

安装命令如下：

```bash
$ npm install --global babel-cli
```

基本用法如下：

```bash
# 转码结果输出到标准输出
$ babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js
# 或者
$ babel example.js -o compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ babel src --out-dir lib
# 或者
$ babel src -d lib

# -s 参数生成source map文件
$ babel src -d lib -s
```

以上代码是在全局环境下安装的，存在两个问题：
1. 如果项目要运行，全局环境必须有 Babel，也就是说项目对环境产生了依赖。
2. 全局安装 Babel，无法支持不同项目使用不同版本的 Babel。

针对上述问题，解决的办法是将 `babel-cli` 安装在项目之中。

```bash
# install
$ npm install -save-dev babel-cli
```

然后，改写 `package.json`。

```json
{
  // …
  “devDependencies”: {
    “babel-cli”: “^6.0.0”
  },
  “scripts”: {
    “build”: “babel src -d lib”
  },
}
```

转码的时候，执行`$ npm run build` 命令即可。

## 三、babel-node

`babel-cli` 工具自带一个 `babel-node` 命令，提供一个支持 ES6 的 REPl环境。它支持 Node的REPL环境的所有功能，而且可以直接运行ES6代码。

```bash
$ babel-node
> (x => x * 2)(1)
```

`babel-node` 命令可以直接运行ES6脚本。将上面的代码放入脚本文件 `es6.js`，然后直接运行。

```bash
$ babel-node es6.js
2
```

`babel-node` 也可以安装在项目中。

```bash
$ npm install --save-dev babel-cli
```

然后改写 `package.json`。
```json
{
		"scripts": {
			"script-name": "babel-node script.js"
		}
}
```

上面的代码中，使用 	`babel-node` 替代 `node`， 这样 `script.js` 本身就不用做任何转码处理。

## 四、babel-register

`babel-register` 模块改写 `require` 命令，为它加上一个钩子。此后，每当使用 `require` 加载 `.js` 、`.jsx` 、 `.es` 和 `.es6` 后缀名的文件，就会先用 Babel进行转码。

```bash
$ npm install -save-dev babel-register
```

使用时，必须首先加载 `babel-register`。
```js

require("babel-register");
require("./index.js");
```

然后就不需要手动对 `index.js` 转码了。

需要注意的是， `babel-register` 只会对 `require` 命令加载的文件转码，而不会对当前文件转码。另外它是实时转码，所以只适合在开发环境使用。

## 五、babel-core

如果某些代码需要调用到Babel的API进行转码，就需要使用 `babel-core` 模块。

安装命令如下：

```bash
$ npm install babel-core --saves
```

然后，在项目中就可以调用  `babel-core`。

```js

var babel = require("babel-core");

// 字符串转码
babel.transform('code();', options);
// => { code, map, ast}

// 文件转码 (异步)
babel.transformFile('filename.js', options, function(err, result) {
		result;  // => {code, map, ast}
});

// 文件转码（同步）
babel.transformFileSync('filename.js', options);
// => {code, map, ast}

// Babel AST转码
babel.transformFromAst(ast, code, options);
// => { code, map, ast }
```

配置对象 `options`,可以参看官方文档 [http://babeljs.io/docs/usage/options/](http://babeljs.io/docs/usage/options/) 。

下面是一个例子。

```js

var es6Code = 'let x = n => n + 1';
var es5Code = require('babel-core')
	.transform(es6Code, {
	presets: ['es2015']
})
.code
// '"use strict";\n\nvar x = function x(n) {\n  return n + 1;\n};'
```
上述代码中， `transform` 方法的第一个参数是字符串，标识需要转换的ES6代码，第二个参数是转换的配置对象。

## 六、babel-polyfill

Babel默认制转换新的JavaScript句法(syntax)，而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如  `Object.assign`）都不会转码。

举例来说，ES6在 `Array` 对象上新增了 `Array.from` 方法。 Babel就不会转码这个方法。如果想让这个方法运行，必须使用 `babel-polyfill`,为当前环境提供一个垫片。

安装命令如下：
```bash

$ npm install --save babel-polyfill
```

然后，在脚本头部，加入如下代码：

```js

import 'babel-polyfill';
// 或者
require('babel-polyfill')
```

## 七、浏览器环境

Babel也可以用于浏览器环境。但是，从Babel 6.0开始，不再直接提供浏览器版本，而是要用构建工具构建出来。如果你没有或者不想使用构建工具，可以通过安装5.x版本的 `babel-core` 模块获取。

```bash
$ npm install babel-core@old
```

运行上面的命令以后，就可以在当前目录的 `node_modules/babel-core`子目录里面，找到`babel` 的浏览器版本`browser.js`和`browser.min.js`。

然后，将下面代码插入网页：

```html
<script src="node_modules/babel-core/browser.js"></script>
<script type="text/babel">
// Your ES6 code
</script>
```

上面代码中，`browser.js` 是Babel提供的转换器脚本，可以在浏览器运行。用户的ES6脚本放在 `script`标签之中，但是要注明 `type="text/babel"`。

另一种方法是使用 [babel-standalone](https://github.com/Daniel15/babel-standalone) 模块提供的浏览器版本，将其插入网页。

```html
<script src=" [https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.4.4/babel.min.js](https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.4.4/babel.min.js) "></script>
<script type="text/babel">
// Your ES6 code
</script>
```

注意：网页中实时将ES6代码转为ES5，对性能会有影响。生产环境需要加载已经转码完成的脚本。

下面是如何将代码打包成浏览器可以使用的脚本，以 `Babel` 配合 `Browserify` 为例。首先，安装 `babelify` 模块：

```bash
$ npm install --save-dev babelify babel-preset-es2015
```
然后，再用命令行转换ES6脚本：

```bash
$ browserify script.js -o bundle.js \ -t [babelify --presets [ es2015 react] ]
```

上面代码将ES6脚本 `script.js`，转为 `bundle.js`，浏览器直接加载后者就可以了。

在 `package.json` 设置下面的代码，就不用每次命令行都输入参数了。

```json

{
	"browserify": {
		"transform": [["babelify", {"presets": ["es2015"]}]]
	}
}
```

## 八、与其他工具配合

这里举两个例子：ESLint和Mocha。

 [ESLint](http://eslint.org/) 用于静态检查代码的语法和风格，安装命令如下：

```bash
$ npm install --save-dev eslint babel-eslint
```

然后，在项目根目录下，新建一个配置文件`.eslint`，在其中加入`parser`字段。

```js
{
  “parser”: “babel-eslint”,
  “rules”: {
    …
  }
}
```

 再在 `package.json` 之中，加入相应的`scripts`  脚本。
```js
{
    "name": "my-module",
    "scripts": {
      "lint": "eslint my-files.js"
    },
    "devDependencies": {
      "babel-eslint": "...",
      "eslint": "..."
    }
  }
```

 [Mocha](http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html) 则是一个测试框架，如果需要执行使用ES6语法的测试脚本，可以修改`package.json` 的`scripts.test`。

```js
"scripts": {
  "test": "mocha —ui qunit —compilers js:babel-core/register"
}
```

上面命令中， `--compilers` 参数指定脚本的转码器，规定后缀名为 `js` 的文件，都需要使用 `babel-core/register`先转码。


---
文章摘自： [http://www.ruanyifeng.com/blog/2016/01/babel.html](http://www.ruanyifeng.com/blog/2016/01/babel.html) 