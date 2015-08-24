<!-- readme.md -->
#在react－router的使用当中使用定制属性

React router的不同组件之间的通信可以通过url路径（params和query两种机制） 或者是使用和flux相同的基于全局变量的方式。 但是有时候还是需要对组件提供更方便的封装机制。

例如， 有一个菜单列表组件， 在两个不同的页面中都有用到， 两者除了菜单的item不同， 其他都是一样的。
```
<ul>
	{ 
		this.props.map((item, index) => {
			return <li key={index}> item </li>;
		})
	}
</ul>
```
在react-router的handler的定义方式里， 并没有提供传入property的方式：
```
<Route name="menu" handler={Menu} />
```
这种情况下， 想要实现Menu组件在不同页面的复用， 初看来有三个办法
1. 通过query参数传递item作为数组
2. 通过类似flux中的store的方式
3. 每次使用该类， 即在外面封装一个class：
```
	var items = [...];
	var OutMenu = React.createClass({
		render: function(){
			return <Menu items={items} />
		}
	});
```

第一种将实现依赖于url, 不仅莫名的增加了url的长度, 不具有扩展性, 也不安全。
第二种的问题是需要确认store中的数据的正确性, 内聚性低， 用多了工程里会有许多莫名其妙的store作为全局变量使用。
看起来第三种方法， 每次使用时在外面封装个类的方法是问题最少的。 但是这个方法最大的问题是会定义许多封装类， 代码太湿（not dry）。

试着将这种定义封装类的方法进行抽象。定义一个工厂类。
```
function propertyFactory(Type, props){
	return React.createClass({
		render: function() { return <Type {...this.props} {...props} /> }
	});

}

var OutMenu = propertyFactory(Menu, items);
```
干净多了！

用这个factory定义的class满足了我在实践中的大部分需求。 我遇到的最大的问题是类（静态）方法无法复制。在react-router中就是willTransitionTo和willTransitionFrom无效。 目前还没想到很好的解决办法， 只能hard code：
```
function propertyFactory(Type, props){
	return React.createClass({
		statics: {
			willTransitionTo: Type.willTransitionTo,
			willTransitionFrom: Type.willTransitionFrom
		},
		render: function() { return <Type {...this.props} {...props} /> }
	});

}
```
遇坑填坑。。。

hope it helpful:)


通过
