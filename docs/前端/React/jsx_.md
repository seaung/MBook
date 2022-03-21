##### JSX简介
JSX是一个javascript的语法扩展，它可以很好地描述UI应该呈现出它应该有交互的本质形式。JSX可能会使人联想到模板语法，但它具有javascript的全部功能。


##### 使用JSX

1. 在JSX中嵌入表达式。在JSX语法中，你可以在大括号中放置任何合法的Javascript表达式。例如:四则运算,函数调用等等。

```jsx
const name = "hello world";
const elem = <h1>Say : { name }</h1>;

ReactDOM.render(
	elem,
	document.getElementById("root")
)

# ----

const user = {
	firstname: "json",
	lastname: "hankong",
};

function printInfo(user) {
	return user.firstname + user.lastname;
}

const elem = <h1>Say: {printInfo(user)}</h1>

ReactDOM.render(
	elem,
	document.getElementById("root")
)
```

2. JSX也是一个表达式。在编译后，JSX表达式会被转为普通的javascript函数调用，并对其取值后得到javascript对象。也就是说，你可以在if或for语句块中使用JSX，将JSX赋值给变量，把JSX当作参数传入，以及从函数返回JSX。

```jsx
function getInfo(user) {
	if(user) {
		return <h1>hello! {user.name}</h1>
	}
	return <h1>hello!</h1>
}
``` 

3. JSX中指定属性。你可以通过使用引号，来将属性值指定为字符串字面量。也可以使用大括号，在属性值中插入一个javascript表达式

```jsx
const elem = <a href="https://www.baidu.com">baidu</a>

const elem = <img src={images.link}></img>
```

Note: 在属性中嵌入javascript表达式时，不要在大括号外面加上引号。你应该仅使用引号或大括号中的一种，不要再同一属性中使用这两种方式。


4. 使用JSX指定子元素。假如一个标签里面没有内容，可以使用`/>`来闭合标签

```jsx
const elem = <img src={images.link} />
```


5. JSX防止注入。React DOM在渲染所有输入内容之前，默认会进行转义。它可以保证在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。


6. JSX表示对象。babel会把JSX转译为React.createElement()函数调用

```jsx
const elem = (
	<h1 className="greeting">
		hello react!
	</h1>
);

const elem = React.createElement(
	"h1",
	{className: "greeting"},
	"hello react!",
)

const elem = {
	type: "h1",
	props: {
		className: "greeting",
		children: "hello react!",
	}
}
```

---
that's all