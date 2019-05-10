---
title: Vue配置引入自己的包文件
date: 2019-05-10 11:08:55
tags:
- javascript
- vue
---

#vue.config.js中配置编译node_modules中es6的包

因为公司后台项目使用前后端分离的方式，前端使用vue；
公司项目比较多，所以在开发过程中，把需要用到的公共 vue 组件和公共代码单独创建了 npm 包，因为我们包里面使用了es6的语法，在我们项目中引入公共包，编译报错，错误原因就是 js 语法报错；
找到解决方案，只要配置那个引入的包，也要使用es6的语法去编辑就好！vue.config.js 配置中添加如下如下：

```js
const path = require('path')

function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
	... // 其他配置
	// 配置你的包也使用 es6 语法解析
	chainWebpack: (config) => {
		config.module
		  .rule('compile')
		  .test(/\.js$/)
		  .include
		  .add(resolve('node_modules/你包的名称'))
		  .end()
		  .use('babel').loader('babel-loader')
	}
}
```
