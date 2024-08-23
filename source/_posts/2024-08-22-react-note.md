---
title: React学习笔记
tags:
  - react
  - web
categories:
  - React
description: 本文主要记录React的学习笔记
abbrlink: 1e564abe
date: 2024-08-22 21:40:35
updated: 2024-08-23 20:21:00
sticky: 1
swiper_index: 2
---

## React

&nbsp;&nbsp;&nbsp;&nbsp;React 是一个用于构建{% nota UI, 用户界面（User Interface）%}的 JavaScript 库，用户界面由按钮、文本和图像等小单元内容构建而成。React 帮助你把它们组合成可重用、可嵌套的组件。从 web 端网站到移动端应用，屏幕上的所有内容都可以被分解成组件。

## 组件

&nbsp;&nbsp;&nbsp;&nbsp;React应用程序由组件组成， 组件是 UI 的一部分， 可以小到一个button，大到整个页面。**React组件是返回标签的JavaScript函数**：

```JavaScript
function MyButton() {
  return <button>I'm a button</button>;
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  )
}
```

## {% nota JSX, JavaScript and XML %}语法

&nbsp;&nbsp;&nbsp;&nbsp;JSX 语法即上面所用的标签语法。 JSX 比 HTML 对标签的要求更为严格，必须闭合标签，如`<br />`，且一个组件只能返回一个 JSX 标签，而不能是多个，若需要返回多个兄弟标签，可以使用`<div>...</div>`或空的`<>...</>`将其包裹住（空白标签不会渲染到 DOM 中）。

{% folding yellow, 空标签为Fragment标签简洁写法 %}

- 在大多数情况下，`<Fragment></Fragment>`可以简写为空的JSX元素`<></>`。
- 如果要传递key给一个`<Fragment>`，则不能简写，必须从`'react'`中导入Fragment且表示为`<Fragment key={yourKey}>...</Fragment>`。

{% endfolding %}

&nbsp;&nbsp;&nbsp;&nbsp;属性名由原 HTML 属性名改为小驼峰命名， 例如绑定点击事件处理函数`onClick`、类名`className`、样式中的属性`backgroundColor`。

## JSX中通过`{}`使用JavaScript

&nbsp;&nbsp;&nbsp;&nbsp;在标签中插入JavaScript变量或表达式值需要使用 `{}` 包裹， 若需要写内联样式则还必须将所有样式以对象（键值对）形式作为style属性的值传入:

```JavaScript
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{ // 外层{}表示使用JS变量，内层{}表示传入的是对象
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

## 组件（函数）的唯一参数--Props

&nbsp;&nbsp;&nbsp;&nbsp; **`props`就是传递给JSX标签的信息。** 上面给`img`标签传入的`className`、`src`、`alt`、`style` 及对应的值就是一种`props`对象，另外还有更为常见的传给组件的`props`，如下面的Avatar组件：

```JavaScript
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', image: './1bX5QH6.jpg' }}
      size={100}
    />
  );
}
// 此处为了显得结构清晰才没有在接收参数时解构，建议直接在参数列表中解构：
// function Avatar({ person, size })
// props = { person:{ name: 'Lin Lanying', image: './1bX5QH6.jpg' }, size: 100 }
function Avatar(props) {
  const { person, size } = props;
  return (
    <img
      className="avatar"
      src={person.image}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

{% folding yellow, 使用展开语法传递props %}

```JavaScript
function Avatar({ person, size })
  const subProps = {
    className: "avatar",
    src: person.image,
    alt: person.name,
    width: size,
    height: size
  }
  return (
    <img {...subProps} />
  );
}
```

{% endfolding %}

&nbsp;&nbsp;&nbsp;&nbsp;事实上，`props` 正是组件的唯一参数！ React 组件函数接受一个参数，即一个 `props` 对象。

## JSX 作为子组件传递

&nbsp;&nbsp;&nbsp;&nbsp;当组件作为双标签使用时， `props` 对象中将多出一个 `children` 属性，其值为组件双标签包裹的部分，实例如下：

```JavaScript
function Card({ children, title }) {
  return (
    <div className="card">
      <div className="card-content">
        <h1>{title}</h1>
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card title="Photo">
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card title="About">
        <p>I'm a paragraph child of Card</p>
      </Card>
    </div>
  );
}
```

## 条件渲染

&nbsp;&nbsp;&nbsp;&nbsp;通常组件会需要根据不同的情况显示不同的内容。在 React 中，可以通过使用 JavaScript 的 `if` 语句、`&&` 和 `? :` 运算符来选择性地渲染 JSX。

- 当不想有任何东西进行渲染时，可以让组件返回 null。
- 当 JSX 中 `{exp && 'exp非空'}` 内变量 `exp` 的值为 `false` 时，JS 表达式的渲染结果为空。

{% note warning flat %}

- 在 JSX 中使用`&&`进行条件渲染时， 左值不能为`number`类型， 因为值为除`false`、`null`、`undefined`外的都会被React渲染，`number`类型值可以通过比较运算符转换为布尔类型。
- `''`空字符串也会被渲染，不过实际效果也是空。

{% endnote %}

## 组件的记忆

- 局部变量无法在多次渲染中持久保存
- 更改局部变量不会触发渲染

&nbsp;&nbsp;&nbsp;&nbsp;`useState Hook` 提供了这两个功能：

- State 变量用于保存渲染间的数据。
- State setter 函数更新变量并触发 React 再次渲染组件。

### 总结

- 当一个组件需要在多次渲染间“记住”某些信息时使用 state 变量。
- State 变量是通过调用 `useState Hook` 来声明的。
- `Hook` 是以 `use` 开头的特殊函数。它们能让你 “hook” 到像 state 这样的 React 特性中。
- `Hook` 可能会让你想起 import：它们需要在非条件语句中调用。调用 `Hook` 时，包括 `useState`，仅在组件或另一个 `Hook` 的顶层被调用才有效。
- `useState Hook` 返回一对值：当前 state 和更新它的函数。
- 你可以拥有多个 state 变量。在内部，React 按顺序匹配它们。
- State 是组件私有的。如果你在两个地方渲染它，则每个副本都有独属于自己的 state。

## 渲染和提交

&nbsp;&nbsp;&nbsp;&nbsp;有两种原因会导致组件的渲染:

1. 组件的初次渲染。
2. 组件（或者其祖先之一）的[状态发生了改变](#)。

### 初次渲染

&nbsp;&nbsp;&nbsp;&nbsp;当应用启动时，会触发初次渲染。 框架和沙箱有时会隐藏这部分代码，但它是通过调用 `createRoot` 方法并传入目标 `DOM` 节点，然后用你的组件调用 `render` 函数完成的：
{% tabs 分栏 %}

<!-- tab index.js -->

```JavaScript
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

<!-- endtab -->

<!-- tab Image.js -->

```JavaScript
export default function Image() {
  return (
    <img
      src="https://i.imgur.com/ZF6s192.jpg"
      alt=""
    />
  );
}
```

<!-- endtab -->

{% endtabs %}

### 状态（state）更新时重新渲染

&nbsp;&nbsp;&nbsp;&nbsp;一旦组件被初次渲染，你就可以通过使用 setter 函数 更新其状态（state）来触发之后的渲染。更新组件的状态会自动将一次渲染送入队列。
