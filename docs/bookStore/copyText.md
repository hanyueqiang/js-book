
## 原生JS实现点击按钮复制
在实际开发中，需要用到复制url路径需求，网上有许多插件可以实现，如`import copy from copy-to-clipboard`插件，同时面临着兼容不同浏览器风险，而原生JS基本不会有兼容性`bug`

需求如图：

![deep icon](./copyText.png)

代码实现：


```

	import { message } from 'antd';
	export const copyText = (text) => {
	  const target = document.createElement('div');
	  target.style.opacity = '0';
	  target.innerText = text;
	  document.body.appendChild(target);
	  try {
	    let range = document.createRange();
	    range.selectNode(target);
	    window.getSelection().removeAllRanges();
	    window.getSelection().addRange(range);
	    document.execCommand('copy');
	    window.getSelection().removeAllRanges();
	    message.success('复制成功');
	  } catch (e) {
	    message.warning('复制失败,请手动复制URL');
	  } finally {
	    target.parentElement.removeChild(target);
	  }
	}

```