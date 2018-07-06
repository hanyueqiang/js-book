## 正则表达式几个方法

|name|描述|
|:-|:-|
|exec|一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回null）。|
|test|一个在字符串中测试是否匹配的RegExp方法，它返回true或false。|
|match|一个在字符串中执行查找匹配的String方法，它返回一个数组或者在未匹配到时返回null。|
|search|一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1。|
|replace|一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串。|
|split|一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的String方法。|

## exec

	var reg = /d(b+)d/g;

    var myArray = reg.exec("cdbbdbsbz-cdbbbbbdbsbz");
    console.log(myArray); //["dbbd", "bb", index: 1, input: "cdbbdbsbz-cdbbbbbdbsbz"]

    console.log(reg.lastIndex); //5

    var myArray = reg.exec("cdbbdbsbz-cdbbbbbdbsbz");
    console.log(myArray); //["dbbbbbd", "bbbbb", index: 11, input: "cdbbdbsbz-cdbbbbbdbsbz"]

    console.log(reg.lastIndex); //18

exec方法和java里的用法差不多。exec方法返回的就是一次匹配到的内容，下标0为这次匹配到的全部字符串，1–2等是括号里的内容。reg对象会维护上次匹配到了哪里，可以调用多次exec方法多次匹配。这样就可以通过括号获取需要的内容了

## match

	var re = /(\w+)\s/g;
    var str = "fee fi fo fum";
    var myArray = str.match(re);
    console.log(myArray); //  ["fee ", "fi ", "fo "] 

这个方法在一个string匹配所有合适的子串，并且把他们都放在返回的数组里，所以这个方法不能取出正则表达式里面括号里面的内容

## replace

	name = "Doe, John";
    console.log(name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1")); //John Doe 

 可以通过1,1,2…使用括号匹配到的内容。

## RegExp

 	var reg = /(\w+)\s/g;
    var reg = new RegExp("(\\w+)\\s", "g");

这两种方法是等价的，注意：①在字符串里面\的转义。②非字符串情况下/作为正则的开始和结束标志。

## 正则表达式标志

|name|描述|
|:-|:-|
|g|全局搜索|
|i|不区分大小写搜索。|
|m|多行搜索。|
|y|执行“粘性”搜索,匹配从目标字符串的当前位置开始，可以使用y标志。|

## 学习文档

[正则表达式 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#.E9.80.9A.E8.BF.87.E5.8F.82.E6.95.B0.E8.BF.9B.E8.A1.8C.E9.AB.98.E7.BA.A7.E6.90.9C.E7.B4.A2)