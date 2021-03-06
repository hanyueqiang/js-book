## 安装dva-cli

	$ npm install dva-cli -g

	$ dva -v //查看安装版本

## 创建新的应用
通过`dva new`创建新应用

	$ dva new dva-quickstart

这会创建 dva-quickstart 目录，包含项目初始化目录和文件，并提供开发服务器、构建脚本、数据 mock 服务、代理服务器等功能。

`cd` 进入 `dva-quickstart` 目录，并启动开发服务器：

	$ cd dva-quickstart
	$ npm start //在浏览器里打开 http://localhost:8000 ，你会看到 dva 的欢迎界面。

## 使用 antd
通过 `npm` 安装 `antd` 和 `babel-plugin-import` 。`babel-plugin-import` 是用来按需加载 `antd` 的脚本和样式

	$ npm install antd babel-plugin-import --save

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

## 编写 UI 组件
随着应用的发展，你会需要在多个页面分享 `UI` 元素 (或在一个页面使用多次)，在 `dva` 里你可以把这部分抽成 `component` 。

我们来编写一个 `ProductList component`，这样就能在不同的地方显示产品列表了。

	import React from 'react'
	import PropTypes from 'prop-types'
	import { Table, Popconfirm, Button } from 'antd'

	const ProductList = ({ onDelete, products }) => {
	    const colums = [
	        {
	            title: "Name",
	            detaIndex: "name"
	        },
	        {
	            title: "Actions",
	            render: (text, record) => {
	                return (
	                    <Popconfirm title="Delete?" onConfirm={() => onDelete(record.id)}>
	                        <Button>Delete</Button>
	                    </Popconfirm>
	                )
	            }
	        }
	    ];
	    return (
	        <Table
	            dataSource={products}
	            columns={colums}
	        />
	    )
	}

	ProductList.propTypes = {
	    onDelete: PropTypes.func.isRequired,
	    products: PropTypes.array.isRequired,
	};

	export default ProductList;

## 定义 Model
完成 `UI` 后，现在开始处理数据和逻辑。

`dva` 通过 `model` 的概念把一个领域的模型管理起来，包含同步更新 `state` 的 `reducers`，处理异步逻辑的 `effects`，订阅数据源的 subscriptions 。

新建 `model models/products.js` ：
	
	export default {
	    namespace: 'products', //全局state的key
	    state: [], //初始值 空数组
	    reducers: {
	        'delete': {
	            'delete'(state, { payload: id }) {
	                return state.filter(item => item.id !== id)
	            }
	        }
	    }
	    //reducers 等同于redux里的reducer 接收action 同步跟新state
	}

然后别忘记在` index.js `里载入他：

	app.model(require('./models/products').default)

## connect 起来

到这里，我们已经单独完成了 `model` 和 `component`，那么他们如何串联起来呢?

`dva` 提供了`connect` 方法。如果你熟悉` redux`，这个` connect `就是` react-redux `的 `connect` 。

编辑` routes/Products.js`，替换为以下内容：

	import React from 'react'
	import { connect } from 'dva'
	import ProductList from './../components/ProductList';
	
	const Products = ({ dispatch, products }) => {
	    function handleDelete(id) {
	        dispatch({
	            type: 'products/delete',
	            payload: id
	        })
	    }
	    return (
	        <div>
	            <h2>List of Products</h2>
	            <ProductList onDelete={handleDelete} products={products} />
	        </div>
	    )
	}
	
	export default connect(({ products }) => ({
	    products
	}))(Products);

最后，我们还需要一些初始数据让这个应用 run 起来。编辑 `index.js`：

	const app = dva({
	    initialState: {
	        products: [
	            { name: 'dva', id: 1 },
	            { name: 'antd', id: 2 }
	        ]
	    }
	});

## 构建应用

	$ npm run build

`build` 命令会打包所有的资源，包含 `JavaScript, CSS, web fonts, images, html `等。然后你可以在 `dist/` 目录下找到这些文件