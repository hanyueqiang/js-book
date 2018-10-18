## React路由的跳转的方法

### React-Router 2.0.0 版本

    import { browserHistory } from 'react-router'
    browserHistory.push('/path')

### React-Router 2.4.0版本以上

`React-Router 2.4.0`版本以上，可以通过`mixin`的方法，使组件增加`this.router`属性，然后进行相应的操作，具体`mixin`代码参考如下:

    import { withRouter } from 'react-router';
    clsss ABC extends Component {
    }
    module.exports = withRouter(ABC);

用过`mixin`的组件，会具有`this.router`的属性，只需要使用`this.props.router.push('/path') `就可以跳转到相应的路由

### React-Router 3.0.0版本以上

`3.0.0`版本以后不需要再手动`mixin`相关的`router`属性，在任何需要跳转的组件中写`this.props.router.push('/path') `就可以跳转到响应的路由了。

### React-Router 4.0版本以上

`React-Router 4.0`对路由进行了改进，`router`属性改为了`history`属性，使用方法还是和`3.0`差不多，任何需要跳转的地方使用`this.props.history.push('/path') `就可以进行跳转了

### 参数的获取

使用`this.props.match.params.xxx `可以获取到当前路由的参数

### this.props.history.push('/path') 路由变化，页面无响应 bug解决办法
