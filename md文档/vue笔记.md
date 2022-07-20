## computed和watch之间的区别

1.computed能完成的共嗯那个，watch都可以完成。

2.watch能完成的功能，computed不一定能完成，例如:watch里可以通过写setTimeout等函数进行异步操作.

```
例如,这样可以间隔1s再改变。
'name'(val) {
	setTimeout(() => {
		console.log(this)
		this.fullName = val + '-' +xxx
	}, 1000);
}
```

## vue里判断写普通还是箭头函数的小原则

1.所有被vue管理的函数，最好写成普通函数。这样this的指向才是vm或者组件实例对象。

2.所有不被vue所管理的函数(定时器的回调函数，ajax的回调函数，promise的回调==)，最好写成箭头函数，这样this的指向才是vm或者组件实例对象。

## 绑定class

### 字符串写法:用于样式类名不确定，需要动态指定

```
<div :class="mood"></div>

data: {
mood: 'style'
}
渲染结果为:class="style"
```



### 数组写法:用于绑定的样式不确定，名字也不确定

```
<div :class="arr"></div>

data: {
 arr: ['a', 'b', 'c']
}
渲染结果为: class="a, b, c"
```



### 对象写法:用于绑定的样式个数不确定，名字也确定,但要动态决定是否使用

```
<div :class="obj"></div>

data: {
obj: {
a: true,
b: false
}
}
渲染结果为: class="a"
```

## 绑定style