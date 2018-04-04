#jQuery事件中的命名空间

使用jQuery绑定事件非常简单， 调用on就可以了， 绑定多个事件也很方便。

```
$('#element')
  .on('click', doSomething)
  .on('click', doSomethingElse);
```

后绑定的事件不会重写之前的， 所以doSomething和doSomethingElse都会执行。
有时可能会因为各种原因(例如节省内存或者验证条件不合法禁止操作)需要移除事件也简单，

```
$('#element').off('click');
```

简单的off就可以移除。 但是要注意的是这样会删除#element上的所有的事件， 但是其实我们只想删除一个事件。如果有事件出发的function的引用也简单

```
$('#element').off('click', doSomething);
```

这样就只有doSomething被移除了， 当#elemgnt上触发了click事件时，doSomething不会执行， 但是doSomethingElse还是会执行。但是实际场景中经常会没有对应function的引用。例如是通过匿名方法定义的事件

```
$('#element').on('click', function () {
	console.log('doSomething');
});
```
匿名函数绑定事件是现实中非常常用的一种方法。另一种常见的情况是事件是在另一个作用域或者文件中绑定的， 这时候一般不容易获取原函数的引用。

jQeury提供的解决方法是通过[命名空间](https://api.jquery.com/event.namespace/)。

```
$("#element")
  .on("click.someNamespace", function() { 
     console.log("doSomething");
   });
```

现在就可以直接用off来取消这个事件了

```
$("#element").off("click.someNamespace");
```

event.namespace的另一个用法是trigger：
```
$("#element").trigger("click.someNamespace");
```

在加上namespace后， 原来的事件还是可以正常移除和触发
```
$("#element").off("click"); // 移除所有的click事件
$("#element").trigger("click"); // 触发所有的click事件
```
可以将namespace理解为class。事实上， 和class一样， 你可以使用多个namespace:
```
$("#element").on('click.namespace1.namespace2', ...);
```