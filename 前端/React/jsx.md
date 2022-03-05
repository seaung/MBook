##### create.Element()的问题

1. 繁琐不简洁
2. 不直观，无法一眼看出所描述的结构
3. 不优雅

```jsx
const v = React.createElement("h1", {"className": "title"}, "hello world")

ReactDOM.render(v, document.getElementById("root"))
```

##### jsx简介

jsx，全称javascript XML是一个 JavaScript 的语法扩展。在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它具有 JavaScript 的全部功能。

jsx优势：声明式语法更直观。与html结构相同，降低学习成本提升开发效率。

##### jsx使用步骤

1. 使用jsx语法创建react元素
2. 使用ReactDOM.render()方法渲染react元素到页面中

```jsx
const title = <h1>hello world</h1>;

ReactDOM.render(title, document.getElementById("root"))
```

Note:

1. jsx不是标准的ECMAScript语法，他是ECMAScript的语法扩展
2. jsx需要使用babel编译处理后才能在浏览环境中使用
3. create-app脚手架中已经默认有该配置，无需手动配置
4. 编译jsx语法的为@babel/preset-react
5. react元素的属性使用驼峰命名法
6. 特殊的属性名: class->className, for->htmlFor, tabindex->tabindex
7. 没有子节点的React元素可以用/>结束
8. 推荐使用小括号包裹jsx,从而避免js中自动插入分号的陷阱

##### jsx中使用javascript表达式

1. 差值表达式

```jsx
<span>{1 + 1}</span>

const word = "hello";

<span>{word}</span>

<span>{5 > 3 ? "hello": "world"}</span>
```

2. 单大括号中可以使用任意的javascript表达式
3. jsx本身也可以是jsx表达式
4. js中的对象是一列外，一般只会应用在style属性中
5. 不能在{}中使用语句，例如if/for等语句

##### jsx列表渲染和样式

通常使用map方法来遍历一个列表或对象

```jsx
const items = {"k1": "v1"....};

const div = items.map((item) => {<li key="{item.k1}">item.k1</li>})
```

key值用来标识属性的唯一性

jsx样式

1. 行内

```jsx
<span style={{ color: "red"}}>hello world</span>
```

2. 类名

```jsx
{# index.jsx #}
<span className="title">hello world</span>
 
{# index.css #}
.title {
    color: "red"
}
```



---

that's all