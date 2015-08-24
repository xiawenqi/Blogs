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
这种情况下， 每个页面都将菜单的item放在路径中或者是通过store传递显然不够优雅。而react－router本身并没有提供传递参数的机制。


