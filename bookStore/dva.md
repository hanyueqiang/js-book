## 安装dva-cli

	npm install dva-cli -g

	dva -v //查看安装版本

## 创建新的应用
通过`dva new`创建新应用

	dva new dva-quickstart

这会创建 dva-quickstart 目录，包含项目初始化目录和文件，并提供开发服务器、构建脚本、数据 mock 服务、代理服务器等功能。

`cd` 进入 `dva-quickstart` 目录，并启动开发服务器：

	cd dva-quickstart
	npm start //在浏览器里打开 http://localhost:8000 ，你会看到 dva 的欢迎界面。

## 使用 antd
通过 `npm` 安装 `antd` 和 `babel-plugin-import` 。`babel-plugin-import` 是用来按需加载 `antd` 的脚本和样式

	npm install antd babel-plugin-import --save

编辑 `.webpackrc`，使 `babel-plugin-import` 插件生效。

	{
	    "extraBabelPlugins": [
	        ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
	    ]
	}

## 定义路由

我们要写个应用来先显示产品列表。首先第一步是创建路由，路由可以想象成是组成应用的不同页面。

新建 `route component routes/Products.js`，内容如下：

	import React from 'react'

	const Products = (props) => (
	    <h2>List of Products</h2>
	)
	
	export default Products

添加路由信息到路由表，编辑` router.js :`

	import Products from './routes/Products'
	
	function RouterConfig({ history }) {
	  return (
	    <Router history={history}>
	      <Switch>
	        <Route path="/" exact component={IndexPage} />
	        <Route path="/products" exact component={Products} /> //add router
	      </Switch>
	    </Router>
	  );
	}