## Web的几种上传方式

### 表单上传

这是传统的form表单上传，使用form表单的input[type=”file”]控件，可以打开系统的文件选择对话框，从而达到选择文件并上传的目的，它的好处是多浏览器兼容，它是web开发者最常用的一种文件上传方式。

	<form method="post" action="http://uploadUrl" enctype="multipart/form-data">
	    <input name="file" type="file" accept="image/gif,image.jpg" />
	    <input name="token" type="hidden" />
	    <input type="submit" value="提交" /> 
	</form>

以下是表单上传几个关键点：

* method="post"： 采用post方式提交数据
 
* enctype="multipart/form- data"：采用multipart格式上传文件，此时request头会显示 Content-Type:multipart/form-data; boundary=—-WebKitFormBoundaryzr34cwJ67R95KQC9
* action：标明上传的服务端处理地址
* type="file"：使用input的file控件上传
如果是多文件批量上传，可以将input[type=”file”]的name属性设置为如：name=”file[]”
* accept属性是HTML5的新属性，它规定了可通过文件,上传提交的文件类型

* 上传的触发事件可以是：input[type=”file”]的onChange触发，也可以由一个独立的按钮的onClick使整个表单提交，此时还可以用input[type="hidden"]带一些其它的参数，比如Token来源验证等等。

* 弊端：提交后刷新页面

### Ajax无刷新上传

Ajax无刷新上传的方式，本质上与表单上传无异，只是把表单里的内容提出来采用ajax提交，并且由前端决定请求结果回传后的展示结果，不用像直接表单上传那样刷新和跳转页面。在这里，我们采用jQuery来作为操作DOM和创建ajax提交的js基础库。
	
	<form>
    	<input id="file" name="file" type="file" />
    	<input id="token" name="token" type="hidden" />
	</form>

	//js代码
	$("#file").on("change", function(){
	  	var formData = new FormData();
	  	formData.append("file", $("#file")[0].files);
	  	formData.append("token", $("#token").val());
	  	$.ajax({
	      url: "http://uploadUrl",
	      type: "POST",
	      data: formData,
	      processData: false,
	      contentType: false,
	      success: function(response){
	              // 根据返回结果指定界面操作
	      }
	  	});
	});

* 我们使用了file控件的change来触发上传事件，当然你也可以使用某个按钮来触发表单提交。提交数据时，用到了FormData对象来发送二进制文件，FormData构造函数提供的append()方法，除了直接添加二进制文件还可以附带一些其它的参数， 作为XMLHttpRequest实例的参数提交给服务端。

* 使用jQuery提供的ajax方法来发送二进制文件，还需要附加两个参数：

* processData: false // 不要对data参数进行序列化处理，默认为true
* contentType: false // 不要设置Content-Type请求头，因为文件数据是以 multipart/form-data 来编码

### Flash上传

很多时候上传的需求要求显示上传进度、中断上传过程、大文件分片上传等等，这时传统的表单上传很难实现这些功能，于是产生了使用Flash上传的方式，它采用Flash作为一个中间代理层，代替客户端跟服务端通信，此外，它也对客户端的文件选择方面拥有更多的控制权，比input[type=”file”] 要大得多。

在这里我使用了jQuery封装好的uploadify插件来进行演示，一般这类插件都自带了上传用的Flash文件，因为跟服务端回传的数据和展示跟客户端的交互，都是Flash文件的接口跟插件来对接的。

### 拖拽上传

拖拽上传的方式，支持的浏览器比较少，因为它用到了HTML5的两个新的属性（API）一个是Drag and Drop,一个是File API。

上传域监听拖拽的三个事件：dragEnter、dragOver和drop，分别对应拖拽至、拖拽时和释放三个操作的处理机制，当然你也可以监听dragLeave事件。

HTML5的File API提供了一个FileList的接口，它可以通过拖拽事件的e.dataTransfer.files来传递的文件信息，获取本地文件列表信息。

File API在HTML5规范中只是草案，在 W3C 草案中，File 对象只包含文件名、文件类型和文件大小等只读属性。但部分浏览器在草案之外提供了一个名为 FileReader 的对象，用以读取文件内容，并且可以监控读取状态，它提供的方法有： “readAsBinaryString” ，”readAsDataURL” ，”readAsText” ，”abort” 等。

### 上传与安全

上传文件时必须做好文件的安全性，除了前端必要的验证，如文件类型、后缀、大小等验证，重要的还是要在后台做安全策略。

这里我列举几个注意点：

* 后台需要进行文件类型、大小、来源等验证
* 定义一个.htaccess文件，只允许访问指定扩展名的文件。
* 将上传后的文件生成一个随机的文件名，并且加上此前生成的文件扩展名。
* 设置上传目录执行权限，避免不怀好意的人绕过如图片扩展名进行恶意攻击，拒绝脚本执行的可能性。

## 下载

我们在下载文件时，一般都是以GET方式下载，但是GET请求有参数长度限制，这时候就可以通过构建form表单以POST请求方式下载
	
	$(function (exports) {
	    function c_downLoad(arg) {
	        var downLoad = {
	            url: arg.url,
	            dataModel: arg.dataModel
	        }
	        downLoad.init = function () {
	            var form = $("<form>");//定义一个form表单
	            form.attr("style", "display:none");
	            form.attr("target", "");
	            form.attr("method", "post");
	            form.attr("action", downLoad.url);//URL
	            var arrFile = JSON.stringify(downLoad.dataModel);
	            if (downLoad.dataModel != null) {
	                var input = $("<input>");
	                input.attr("type", "hidden");
	                input.attr("name", "arrFile");
	                input.attr("value", arrFile);
	                form.append(input);
	            }
	            $("body").append(form);//将表单放置在web中
	            form.submit();//表单提交 
	            form.remove();//移除该临时元素
	        }
	        return downLoad;
	    }
	    exports.i_downLoad = function (arg) {
	        var u = c_downLoad(arg);
	        u.init();
	    }
 
	}(window));

### 使用window.open

	window.open("down/globe.rar"); 

window.open()的原理是将文件打开，之所以你能保存excel是因为浏览器不能直接显示excel的数据，所以会自动给你下载，而浏览器可以直接显示text的数据所以他就不会自动下载，而是打开一个新窗口显示text中的内容