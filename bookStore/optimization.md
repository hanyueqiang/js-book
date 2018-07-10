## 你在前端性能优化方面有什么心得？

### 项目中大多使用React + Webpack：

####  `React`

* `{...this.props}` 不要滥用，请只传递`component`需要的`props`
* `props` `state` 尽可能的简单，扁平化，`props`传的太多或者传得太深，都会加重`shouldComponentUpdate`里面的数据比较负担
* 多使用`Functional Component` （不需要使用组件的生命周期）和` PureComponent` （与普通Component的区别在于重写了`shouldComponentUpdate`方法，会对`state`和`props`进行浅比较）
* 建议引入`Immutable.js`，可以配合`shouldComponentUpdate`方法，手动改变返回值
* `::this.handle` 需要绑定`this`的方法，一律放在`construtor`里面，放在`render`里面每次渲染都会绑定一次
* 合理的组件拆分，避免一个组件过于庞大

#### `Webpack`

* 公共代码提取，`webpack <= 3` 版本可以使用`CommonsChunkPlugin`插件
* 代码分割（异步加载），`webpack 2` 可以使用`require.ensure()`，`webpack 3`可以使用`() => import()`

## 在类似webpack／gulp的自动化工具使用上，你有什么优化经验／心得？

## 如果网站首页加载速度很慢，你会怎样定位问题？