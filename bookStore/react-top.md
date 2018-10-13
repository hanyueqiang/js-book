### 滚动条返回顶部
在 React 组件间进行页面跳转后，发现页面的位置并不在页面顶部，而是在页面跳转前的位置。就是说浏览器的滚动条并没有回到顶部的位置。

    class MyComponent extends Component {
        componentDidMount() {
            this.node.scrollIntoView();
        }
        render() {
            return <div ref={node => this.node = node} />
        }
    }

    如何还不行就强行：
    componentWillMount(){
        document.getElementById('App').scrollIntoView(true);//为ture返回顶部，false为底部
    }