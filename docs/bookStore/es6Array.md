## find 
`find`参数为回调函数，回调函数接受3个参数，值`x`,索引`i`,原数组`arr`,回调函数默认返回值`x`.
	
	let arr = [1,2,3,4];

	arr.find(function(x) {
		return x <= 2;
	}) 
	// 结果返回第一个符合条件的值 1

## findIndex
findIndex和find差不多，不过默认返回的是索引。

	let arr = [1,2,3,4];
	arr.find(function(x){
		return x <= 2;
	}) 
	// 返回第一个符合条件的x值的索引 0

## includes
includes函数与string的includes一样，接收2参数，查询的项以及查询起始位置。返回布尔类型；

	let arr=[1,2,3,4];

	arr.includes(2); // 结果true，返回布尔值
	arr.includes(20); // 结果：false，返回布尔值
	arr.includes(2,3) //结果：false，返回布尔值

## keys
对数组索引的遍历
	
	let arr=[1,2,234,5,6];

	for(let a of arr.keys()){
	    console.log(a)
	}//结果：0,1,2,3,4  遍历了数组arr的索引

## values
对数组项的遍历

	let arr=[1,2,234,5,6];

	for(let a of arr.values()){
	    console.log(a)
	}//结果：1,2,234,5,6 遍历了数组arr的值

## entries
对数组键值对的遍历

	let arr=['w','b'];

	for(let a of arr.entries()){
	    console.log(a)
	}//结果：[0,w],[1,b]
	for(let [i,v] of arr.entries()){
	    console.log(i,v)
	}//结果：0 w,1 b

## fill
fill方法改变原数组，当第三个参数大于数组长度时候，以最后一位为结束位置。

	let arr=['w','b'];

	arr.fill('i')//结果：['i','i']，改变原数组
	arr.fill('o',1)//结果：['i','o']改变原数组,第二个参数表示填充起始位置
	new Array(3).fill('k').fill('r',1,2)//结果：['k','r','k']，第三个数组表示填充的结束位置

## Array.of()
Array.of()方法永远返回一个数组，参数不分类型，只分数量，数量为0返回空数组。

	Array.of('w','i','r')//["w", "i", "r"]返回数组
	Array.of(['w','o'])//[['w','o']]返回嵌套数组
	Array.of(undefined)//[undefined]依然返回数组
	Array.of()//[]返回一个空数组

## copyWithin
copyWithin方法接收三个参数，被替换数据的开始处、替换块的开始处、替换块的结束处(不包括);copyWithin(s,m,n).

	["w", "i", "r"].copyWithin(0)//此时数组不变
	["w", "i", "r"].copyWithin(1)//["w", "w", "i"],数组从位置1开始被原数组覆盖，只有1之前的项0保持不变
	["w", "i", "r","b"].copyWithin(1,2)//["w", "r", "b", "b"],索引2到最后的r,b两项分别替换到原数组1开始的各项，当数量不够，变终止
	["w", "i", "r",'b'].copyWithin(1,2,3)//["w", "r", "r", "b"]，强第1项的i替换为第2项的r

## Array.from()
Array.from可以把带有lenght属性类似数组的对象转换为数组，也可以把字符串等可以遍历的对象转换为数组，它接收2个参数，转换对象与回调函数

	Array.from({'0':'w','1':'b',length:2})//["w", "b"],返回数组的长度取决于对象中的length，故此项必须有！
	Array.from({'0':'w','1':'b',length:4})//["w", "b", undefined, undefined],数组后2项没有属性去赋值，故undefined
	Array.from({'0':'w','1':'b',length:1})//["w"],length小于key的数目，按序添加数组
	
	let divs=document.getElementsByTagName('div');

	Array.from(divs)//返回div元素数组

	Array.from('wbiokr')//["w", "b", "i", "o", "k", "r"]

	Array.from([1,2,3],function(x){
	       return x+1
	})	//[2, 3, 4],第二个参数为回调函数