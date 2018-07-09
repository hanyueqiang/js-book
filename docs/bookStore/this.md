## this

`JavaScript` 的 `this` 总指向一个对象，具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。

`this` 的指向大致可以分为以下 4 种：

* 作为对象的方法调用
* 作为普通函数调用
* 构造器调用
* `Function.prototype.call` 和 `Function.prototype.apply` 调用

### 1. 作为对象的方法调用

当函数作为对象的方法被调用时，`this` 指向该对象：

	var obj = {
	    name: 'Jack',
	    getName: function () {
	        alert(this === obj);  // true
	        alert(this.name);  // "Jack"
	    }
	}
	
	obj.getName();

### 2. 作为普通函数调用

当函数不作为对象的属性被调用时，也就是我们常说的普通函数方法，此时的 `this` 总是指向全局对象。在浏览器的 `JavaScript` 中，这个全局对象是 `window`对象

	window.name = 'globalName';
	
	var getName = function () {
	    return this.name;
	}
	
	console.log( getName() );  // "globalName"

看一个稍微复杂些的例子：

	window.name = 'globalName';
	
	var obj = {
	    name: 'Jack',
	    getName: function () {
	        return this.name;
	    }
	}
	
	var getName = obj.getName;
	
	console.log( getName() );  // "globalName"

严格模式下，此时的 `this` 不会指向全局对象，而是 `undefined`。

### 3. 构造函数调用

当用 `new` 操作符调用函数时，该函数总会返回一个对象，通常情况下，构造函数里的 `this` 就指向返回的这个对象：

	var MyClass = function () {
	    this.name = 'Jack';
	}
	
	var obj = new MyClass();
	
	console.log( obj.name );  // "Jace"

### 4. call 或者 apply 调用

`call` 或 `apply` 可以动态地改变传入函数的` this`：

	var obj1 = {
	    name: 'Jack',
	    getName: function () {
	        return this.name;
	    }
	}
	
	var obj2 = {
	    name: 'anne'
	}
	
	console.log( obj1.getName() );  // "Jack"
	console.log( obj2.getName.call( obj1 ) );  // "anne"

例子：丢失的 this`
`document.getElementById()` 这个方法名有点过长，我们尝试用一个短的函数名来代替它：

	var getId = function (id) {
    	return document.getElementById( id );
	}

我们也许思考过为什么不能用下面这种更简单的方式：

	var getId = document.getElementById;

如果你尝试使用 `getId()` 获取` DOM `元素，会看到浏览器会抛出一个异常。这是因为 `document.getElementById()` 方法的内部实现中需要用到 `this`。这个` this` 本来被期望指向` document`，当 `getElementById `方法作为` document `对象的属性被调用时，方法内部的 `this `确实是指向` document` 的。

但当用 `getId `来引用 `document.getElementById `之后，再调用 `getId`，此时就成了普通函数调用，函数内部的 `this `指向了 `window`，而不是原来的` document`。

现在尝试利用 call 来修正 this：

	var getId = document.getElementById;
	var dom = getId.call(document, 'id');