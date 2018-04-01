# 前端代码风格建议

## 为什么要有代码风格

一门计算机语言并不仅仅是计算机去执行的一种方式,更重要的,它是一种表述有关方法学的思想的新颖的形式化媒介。程序必须写出来是供人们阅读的, 偶尔地送到计算机上去执行。在代码的生命周期中, 初期的开发只占20%, 另外的80%都在维护。一段代码经常会被反复阅读,调用,修改, 重构, 这些时候我们写的代码都会被阅读。所以代码的可读性是衡量代码质量的重要属性。 在编写代码的过程中, 最好时刻保持这样的意识: 这段代码我自己在几个星期之后还能不能看得懂了, 别人能不能读懂这些代码？

代码风格很大的决定了代码的可读性。 整个项目中应该保持代码的统一风格: 这样读者就可以集中精力关注代码做了什么事情, 而非他是怎么样写代码的。 

一般来说, **遵循什么样的代码的风格并不重要, 例如操作符两端留不留空格, 字符串使用单引号还是双引号, 重要的是写之前看看周围的代码并保持一致**: 如果别人的运算符两边都有空格, 就在操作符两边加上空格, 如果别人的字符串是用的单引号, 就使用单引号。


## js文件
js代码都写在.js文件里面。 除非必须, 避免在页面中直接使用script标签。


## 缩进
使用四空格缩进。用tab的时候各个操作系统不统一。可以通过文本编辑器配置。


## 行长度
避免行长度超过80个字符。当语句的长度超过80个字符时, 分解语句。在运算符后面折行, 或者在逗号后面更好。折后的行应该缩进8个字符。


## 注释
多写注释。方法名写的不够清晰的注明方法的作用。分支如果别人可能读不懂, 说明每个分支的情况。变量, 特别是全局变量(避免全局变量)可能造成别人读不懂的时候注释说明。

维护注释。代码重构了之后如果原来的注释和代码不一致了应及时维护, 和代码不一致的注释会造成更多疑惑。

确保注释有意义。告诉读者（很多时候是作者自己）他一眼看不出来的信息, 帮读者读懂代码。不用这种注释浪费读者的时间:

```
// 将i的值设为0
i = 0; 
```

## 空格
运算符两边保留空格

```
foo = bar + zoo;
```

参数之间的逗号后插入一个空格, 
```
callback(arg1, arg2);
[elem1, elem2];
```
括号的开头和结尾不保留空格。
```
// 不采用这种空格方式
callback( arg1, arg2 );
[ elem1, elem2 ];
```

## 花括号
花括号开头在语句当前行, 不另起一行

```
// 错误
function foo()
{

}
// 正确
if(test) {
	
}
```

由于Ken Tompsen的B语言编译器(C语言前身)允许两种格式都编译通过， 大家一直在争论花括号应该在当前行还是另起一行开始。 如果Ken当年规定只有一种语法是正确的, 另外一种写法是语法错误, 那程序员的世界肯定会安宁许多。K&R C里的demo花括号都是另起一行, 所以很多语言建议另起一行。 但是JS里建议把花括号的开头保留在这一行:

```
// 错误！！！
return
{
	key: value
};

```
由于分号的自动补全, 这样的return的是undefined。 所以return后的花括号开头必须在当前行。 为了程序中花括号风格保持一致， 建议花括号都从当前行开始。

## 字符串
优先单引号于双引号

```
'"Hey", he said, "how are you?"'
```

除非必须, 避免在js中用字符串拼接html, 用script标签。

纯Html标签用
```<script type="text/html">```
模板用
```<script type="text/template">```

这两个有什么区别吗？ 没有。 只要不是type="text/javascript"的都不会解析成js, 这里只是作为字符串的载体。

另外, 引用脚本文件的标签也不该加上type="text/javascript"。 这里是指定资源的MIME类型, 但是MIME应该由服务器指定, 而非script标签。

## 变量申明
Js中的变量的作用域是以function为作用域, 而不是块。 (推荐阅读[变量提升](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html))

所有未声明而直接使用的变量都会变成全局变量。

在function的第一个语句声明变量, 使用单var模式:
```
var x, y, z;
```

避免使用全局变量,全局变量让程序很难阅读。 如果一定要用, 显式的初始化和使用: 
```
window.gloablVariable = { key: 'initial value' };
window.gloablVariable.key = 'altered value';
```


## 函数声明
避免函数表达式, 容易和变量混淆。 尽量函数声明:
```
function foo(){

}
```

除非这种情况:

```
var bar = (function(){
	var tmp = '';
	
	return function(x, y){
		
	};
})();
```


## 命名（Naming）
用英文单词作为函数名和变量名, 不要拼音。

统一驼峰规则。普通变量用小驼峰： thisIsANormalVariable
构造函数用大驼峰： ThisIsAClass。 因为除了这一点, 我们没办法区分构造函数和普通function

变量使用修饰语+名词: paintedColor,  firstElement。

数组使用复数:  installedKeys。

函数动词开头: renderPage, getKeys, setName

函数返回布尔值的is开头: isIntalled, isEmpty(注意是函数不是变量, 变量此处叫installed, empty)。

确保变量名不增加读者的阅读成本。


### return语句
return的值不用小括号包起来。 返回值应该在return的同一行, 避免分号插入。

### if语句
如果在else的后面没有更多的代码, 可以考虑提前返回, 精简代码。例如值校验。

```
if(value === '') {
	// logic
	return;
}
if(value > MAX) {
	return;
}
// formal logic
```

避免这样写:
```
if(value === '') {
	// …
} else {
	if(value > MAX) {
		// …
	} else {
		// formal logic
		// 嵌套过深
	}
}
```

### Switch语句
分支不需要缩进。case不是语句, 所以可以不遵循语句的缩进规则, 也可以减少缩进。
每个分支都以break, return, 或者是throw语句结尾, 处理后及时跳出, 否则至少会被default分支捕捉到。

```
switch (expression) {
case expression:
    statements
default:
    statements
}
```

### Try语句
采用如下形式
```
try {
    statements
} catch (variable) {
    statements
}

try {
    statements
} catch (variable) {
    statements
} finally {
    statements
}
```

关于异常: 异常是程序员的好朋友, 它帮我们排除错误。 除非清楚要捕捉的异常是什么, 否则不应该用catch把异常吞了。 有异常说明有错误, 利用异常栈找出代码中的错误。

如果想保证API以正确的方式被调用, 可以在参数不正确的时候把异常throw出来。

总之, throw异常的时候开放一点, 但是在catch的时候保守一点。

### Continu语句
会混淆控制流, 避免使用

### With语句
不用

## 其他建议

### {}和[]
用{} 而非new Object(); 用[] 而非 new Array()。

### 赋值语句
避免在条件语句中赋值。
```
if (a = b) {
```
这是想赋值还是本来想写
```
if (a == b) {
```
漏了等号？

### === 和 !==
用===和！== 。 == 和！= 不比较类型, 会试图进行类型转换。


## 参考文档
1. Douglas Crockford, [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html])
2. Sun, [Java Code Conventions](http://javascript.crockford.com/javacodeconventions.pdf)
3. Addy Osmani, [JavaScript Style Guides And Beautifiers](http://addyosmani.com/blog/javascript-style-guides-and-beautifiers/)
4. Google, [Google JavaScript Style Guide](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
5. jQuery community, [jQuery JavaScript Style Guide](https://contribute.jquery.org/style-guide/js/)
6. Harold Abelson and Gerald Jay Sussman, [SICP, Preface to the First Edition](http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-7.html)
