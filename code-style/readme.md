# 前端代码风格建议

## 为什么要有代码风格

一门计算机语言并不仅仅是计算机去执行的一种方式,更重要的,它是一种表述有关方法学的思想的新颖的形式化媒介。程序必须写出来是供人们阅读的, 偶尔地送到计算机上去执行。在代码的生命周期中, 初期的开发只占20%, 另外的80%都在维护。一段代码经常会被反复阅读,调用,修改, 重构, 这些时候我们写的代码都会被阅读。所以代码的可读性是衡量代码质量的重要属性。 在编写代码的过程中, 保持这样的意识非常重要: 我写的这段代码我自己在几个星期之后还能不能看得懂了, 别人能不能读得懂我写的代码？

代码风格很大的决定了代码的可读性。 整个项目中的代码保持统一的风格非常重要: 这样读者就可以集中精力关注代码做了什么事情, 而非他是怎么样写代码的。 不一致的风格至少会导致读代码的过程被打断: "嗯, 这里的命名风格和其他地方不一致, 是什么原因呢？" 或者是更糟, 根本就读不懂。而一致的代码风格可以让整个项目看起来像是一个人写的。

在这里需要强调的是, **遵循什么样的代码的风格并不重要, 例如操作符两端留不留空格, 字符串使用单引号还是双引号, 这些规则是次要的, 但是请注意在写之前看看你周围的代码并保持一致**: 如果别人的运算符两边都有空格, 请在你的操作符两边加上空格, 如果别人的字符串是用的单引号, 请使用单引号。本文给出的是一些风格上的建议, 但是并不是强制的, 很多规则在社区怎么执行争论已久, 我也并不哪项规则的支持者, 请将它修改成适合自己的, 并且在整个项目中都严格遵守。


## js文件
js代码都写在.js文件里面。 除非必须, 避免在页面中直接使用script标签。

体育组的代码都放在由seajs引用的一个主文件中, 都是一个页面一个文件。模块通过require引用。


## 缩进
使用四空格缩进。用tab的时候各个操作系统不统一。可以通过文本编辑器配置。


## 行长度
避免行长度超过80个字符。当语句的长度超过80个字符时, 分解语句。在运算符后面折行, 或者在逗号后面更好。折后的行应该缩进8个字符。


## 注释
多写注释。方法名写的不够清晰的注明方法的作用。分支如果别人可能读不懂, 说明每个分支的情况。变量, 特别是全局变量(避免全局变量)可能造成别人读不懂的时候注释说明。

维护注释。代码重构了之后如果原来的注释和代码不一致了请注意维护,否则后面的人看见和代码不一致的注释, 会造成更多疑惑。

确保注释有意义。告诉读者他一眼看不出来的信息, 帮别人(很多时候是过段时间的自己)读懂你的代码。不用这种注释浪费读者的时间:

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
if(test){
	
}
```

关于花括号到底是在当前行还是另起一行还是在当前行的问题程序员们已经争议了一万年。 开始于Ken Tompsen的B语言编译器(C语言前身)允许两种格式都编译通过。 如果Ken当年规定只有一种语法是正确的, 另外一种写法是语法错误, 那程序员的世界肯定会安宁许多。K&R C里的demo花括号都是另起一行, 所以很多规范都是另起一行。 但是js程序员们有充足的理由把花括号的开头保留在这一行:

```
// 错误！！！
return
{
	key: value
};

```
因为分号的自动补全, 这样的return的是undefined。 所以return后的花括号开头必须在当前行。 为了我们可爱小小程序的花括号一致性, 我们决定： 所有花括号, 都在当前行开始！ Hooray!

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

避免使用全局变量,全局变量让程序很难阅读。我自己的经验是只要愿意, 总是可以不用全局变量。每次看到别人的全局变量的时候（特别是没命名好的）, 我都得在整个文件里来回找变量的声明才能知道这个变量什么意思。 如果一定要使用, 显示的初始化和使用: 
```
window.gloablVariable = { key: 'initial value' };
window.gloablVariable.key = 'altered value';
```

这样我们读到未声明的变量时就不会疑惑: 这是作者有意为之使用的全局变量呢,还是漏了声明而意外造成的全局变量？


## 函数声明
避免函数表达式, 容易和变量混淆。 尽量函数声明:
```
function foo(){

}
```

除非要显示的利用闭包:

```
var bar = (function(){
	var tmp = '';
	
	return function(x, y){
		
	};
})();
```

main函数都放在define函数体的尾部.把你的逻辑拆分成独立的单元, 写在函数里面, 然后再在main函数里面调用
```
function yourlogic(){
	// 逻辑写在这里
}

$(function(){
	yourlogic();
	// 不要把逻辑直接写在main函数里面, 不好阅读！
});
```


## 命名
用英文单词作为函数名和变量名, 不要拼音。

统一驼峰规则。普通变量用小驼峰： thisIsANormalVariable
构造函数用大驼峰： ThisIsAClass。 因为除了这一点, 我们没办法区分构造函数和普通function

变量使用修饰语+名词: paintedColor,  firstElement。

数组使用复数:  installedKeys。

函数动词开头: renderPage, getKeys, setName

函数返回布尔值的is开头: isIntalled, isEmpty(注意是函数不是变量, 变量此处叫installed, empty)。

关于命名还有一点, 不要用my。除非是页面上的我的空间之类的功能。一些博客和书的作者为了省事, 经常有这样的一些命名， myTemplate, myClass什么的, 但是不要在工程中这么做。每次看到工程里的myRegion, myKey, myTemplate都很纠结。 my不能给出关于这个变量的具体信息,试着换个更有意义的： keyRegion, registeredKey, availableKeys, keyItemTemplate。

确保你的变量名不增加读者的阅读成本。


## 语句
一行一个语句, 用分号结尾。都习惯了, 加上吧。

### return语句
Return的值不要用小括号包起来。 返回值应该在return的同一行, 避免分号插入。

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

避免这种代码:
```
if(value === '') {
	// …
} else {
	if(value > MAX) {
		// …
	} else {
		// formal logic
		// 这里嵌套过深, 不利阅读
	}
}
```

### Switch语句

分支不需要缩进。case不是语句, 所以可以不遵循语句的缩进规则, 也可以减少缩进。
每个分支都以break, return, 或者是throw语句结尾, 处理后及时跳出。 否则至少会被default分支捕捉到。

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

关于异常: 异常是程序员的好朋友, 它帮我们排除错误。 除非清楚要捕捉的异常是什么, 否则不要用catch把异常吞了。 有异常说明有错误, 利用异常栈找出自己的错误。

如果想保证你写的API以正确的方式被调用, 可以在参数不正确的时候把异常throw出来。

总之, throw异常的时候可以开放一点, 但是在catch的时候则应该保守点。

### Continu语句
会混淆控制流, 避免使用

### With语句
不要用。

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
