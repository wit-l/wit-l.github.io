---
title: React学习笔记
tags:
  - react
  - web
categories:
  - React框架
description: 本文主要记录React的学习笔记
abbrlink: 1e564abe
date: 2024-08-22 21:40:35
updated: 2024-08-25 21:58:00
sticky: 1
swiper_index: 2
cover: https://tuchuang.voooe.cn/images/2024/08/17/one_room-1-1_1920x1080.webp
---

## React 简要介绍

&nbsp;&nbsp;&nbsp;&nbsp;React 是一个用于构建{% nota UI, 用户界面（User Interface）%}的**JavaScript 库**，用户界面由按钮、文本和图像等小单元内容构建而成。React 帮助你把它们组合成可重用、可嵌套的组件。从 web 端网站到移动端应用，屏幕上的所有内容都可以被分解成组件。

{% note info flat %}
React和Vue不同，严格来说并非一个框架。生产级的React框架有：[{% nota Next.js, 它的页面路由是一个全栈的React 框架，由Vercel维护 %}](https://nextjs.org)、[{% nota Remix, 一个具有嵌套路由的全栈式React框架 %}](https://remix.run)、[{% nota Gatsby, 一个快速的支持CMS的网站的React框架 %}](https://www.gatsbyjs.com/)、[{% nota Expo, 用于原生应用，可以让你创建具有真正原生 UI 的应用，包括 Android、iOS，以及 Web 应用 %}](https://expo.dev/)等。
{% endnote %}

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

{% folding yellow, 使用展开语法传递 props %}

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
- 当 JSX 中 `{exp && 'exp非空'}` 表达式左值为 `false` 时，JS 表达式的渲染结果为空。

{% note warning flat %}

- 在 JSX 中使用`&&`进行条件渲染时， 左值不能为`number`类型， 因为值为除`false`、`null`、`undefined`外的都会被React渲染，`number`类型值可以通过比较运算符转换为布尔类型。
- `''`空字符串也会被渲染，不过实际效果也是空。

{% endnote %}

## 组件重渲染间的记忆

&nbsp;&nbsp;&nbsp;&nbsp;组件通常需要根据交互更改屏幕上显示的内容。输入表单应该更新输入字段，单击轮播图上的“下一个”应该更改显示的图片。组件需要“记住”某些东西：当前输入值、当前图片。在 React 中，这种组件特有的记忆被称为 `state`。

&nbsp;&nbsp;&nbsp;&nbsp;和原生 JS 不同，React 有以下特点：

1. 更改局部变量不会触发页面重新渲染。因此，即使更改了局部变量，也不会更新到屏幕上。
2. 局部变量无法在多次渲染中持久保存。当React重新渲染组件时，它会从头开始渲染而不会考虑之前对局部变量的任何更改。 因此，组件内的局部变量将会被初始化为最初的值。

{% folding yellow, 为什么局部变量在渲染后就变回初值了？ %}

React 组件内局部变量的生命周期仅存在于渲染过程中，渲染结束后就被销毁。下次渲染时，组件函数会再次执行以创建新的局部变量。

{% endfolding %}

&nbsp;&nbsp;&nbsp;&nbsp;尽管局部变量无法在多次渲染中保存，但是 React 保留了组件实例及其状态（如`useState`或`useReducer`等钩子），并在每次渲染时保持这些状态；以此方式保留上次渲染时对数据的更改，供下次渲染时更新组件使用。

{% folding yellow, 组件实例的生命周期 %}

**函数组件**：每次渲染时，React会调用函数组件并重新计算其JSX输出，但它不会丢弃已存在的状态和副作用（`useEffect`等）。
React通过“Virtual DOM”来高效地更新UI，而不是每次都销毁并重新创建整个组件树。
**状态和副作用**：组件的状态（通过`useState`等）会在前后两次渲染之间保持不变。副作用（通过`useEffect`等设置的）也会根据依赖项来决定是否重新执行。

{% endfolding %}

&nbsp;&nbsp;&nbsp;&nbsp;要使用新数据更新组件，就需要做到以下两点：

1. **保留**渲染之间的数据。
2. **触发** React 使用新数据重新渲染组件。

&nbsp;&nbsp;&nbsp;&nbsp;为此，React 提供了 `useState` Hook，它提供以下两个功能：

- **`state`变量**用于保存渲染间的数据。
- **`state setter`函数**更新变量并触发 React 再次渲染组件（处理事件结束后渲染）。

`useState` 语法： `const [value, setValue] = useState(initialValue);`

&nbsp;&nbsp;&nbsp;&nbsp;`useState` 函数返回一个包含两个元素的数组，其中第一个元素为本次渲染过程中状态的**快照**，第二个值为更改内部状态的函数（更新下一次渲染中的快照）。

{% note info flat %}

{% nota `initialValue` 为初次渲染时使用的初始值, 首次渲染的快照中value就是initialValue本身，重渲染后的值取决于使用setValue修改时传入的值 %}，初始值可以是简单数据类型、引用类型（数组、字符串、对象等）。
**注意：不要直接在 `value` 和 `initialValue` 上进行修改！！！**

{% endnote %}

{% folding yellow, 关于Hook %}

- Hook即钩子，在React中，所有以“use”开头的函数都被称为Hook。
- **Hooks —— 以 `use` 开头的函数——只能在组件或自定义 Hook 的最顶层调用。**
- 不能在条件语句、循环语句或其他嵌套函数内调用 Hook。
- Hook 是函数，但将它们视为关于组件需求的无条件声明会很有帮助。
- 在组件顶部 “use” React 特性，类似于在文件顶部“导入”模块。

{% endfolding %}

&nbsp;&nbsp;&nbsp;&nbsp;使用`useState`有以下几点需要注意：

- 当一个组件需要“记住”某些信息供重渲染后使用，此时应该使用 `state` 变量。
- `useState` Hook 返回一对值（数组类型）：当前 `state` 和更新它的函数 `setState`。
- 一个组件中可以定义多个 `state`。
- 只能在组件或自定义 Hook 的最顶层调用。
- 每个组件实例的 `state` 是隔离且私有的。也就是说，渲染同一个组件两次，每个副本都有独立的`state`，互不影响。
- 不要直接更改 `state` 和传入 `useState` 函数的初始化值，这无法触发组件重渲染。`useState` 返回的数组仅仅是本次渲染所使用的一个快照版本，而非其本体；因此，在渲染过程中，对 `state` 做出的更新只能在下次渲染时生效，本次渲染过程中的 `state` 不会变化。

{% folding yellow, React如何区分多个state？以及为什么useState只能在顶层调用？这涉及到 useState 的实现原理 %}

```JavaScript
// 按顺序存储多个state到数组中
let state = [];
let setters = [];
let firstRun = true;
let cursor = 0; // 每次重渲染时cursor置0，但上面三个保持不变
// 以上全局变量在useState中，私有于声明该state的组件实例
function createSetter(cursor) {
  return function setterWithCursor(newVal) {
    state[cursor] = newVal;
    // 触发组件重渲染
  };
}
// This is the pseudocode for the useState helper
export function useState(initVal) {
  if (firstRun) {
    state.push(initVal);
    setters.push(createSetter(cursor));
    firstRun = false;
  }
  // 按cursor顺序取出对应的value和setter的快照
  const setter = setters[cursor];
  const value = state[cursor];

  cursor++;
  return [value, setter];
}

// Our component code that uses hooks
function RenderFunctionComponent() {
  // state必须是有序的，所以不能在条件语句、循环语句、嵌套语句中使用Hook
  const [firstName, setFirstName] = useState("Rudi"); // cursor: 0
  const [lastName, setLastName] = useState("Yardley"); // cursor: 1

  return (
    <div>
      <Button onClick={() => setFirstName("Richard")}>Richard</Button>
      <Button onClick={() => setFirstName("Fred")}>Fred</Button>
    </div>
  );
}

// This is sort of simulating Reacts rendering cycle
function MyComponent() {
  cursor = 0; // resetting the cursor
  return <RenderFunctionComponent />; // render
}

console.log(state); // Pre-render: []
MyComponent();
console.log(state); // First-render: ['Rudi', 'Yardley']
MyComponent();
console.log(state); // Subsequent-render: ['Rudi', 'Yardley']
// click the 'Fred' button
console.log(state); // After-click: ['Fred', 'Yardley']

```

{% endfolding %}

## 渲染和提交

&nbsp;&nbsp;&nbsp;&nbsp;组件必须被React渲染后才能显示到屏幕上。在React应用中，一次屏幕更新都会发生以下三个步骤：

1. 触发一次渲染。
2. 渲染组件。
3. 提交到 DOM。

### 触发渲染

&nbsp;&nbsp;&nbsp;&nbsp;以下两种情况下组件将被渲染:

1. 组件的初次渲染。
2. 组件（或者其祖先之一）的内部状态发生了改变引起的组件重渲染。

#### 初次渲染

&nbsp;&nbsp;&nbsp;&nbsp;当应用启动时，会触发初次渲染。框架和沙箱有时会隐藏这部分代码，但它是通过调用 `createRoot` 方法并传入目标 DOM 节点，然后用你的组件调用 `render` 函数完成的：
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

#### 状态（state）更新时的**重新渲染**

&nbsp;&nbsp;&nbsp;&nbsp;初次渲染后可以通过使用 `setter` 函数更新其状态（`state`）来触发重新渲染。更新组件的状态会自动将一次渲染送入队列。

### 渲染组件

&nbsp;&nbsp;&nbsp;&nbsp;触发渲染后，React 会调用你的组件来确定要在屏幕上显示的内容。

- 在进行初次渲染时, React 会调用根组件（的渲染方法 `render`），并递归地渲染所有子组件，再生成各自的虚拟 DOM节点，最终生成虚拟 DOM 树。
- 对于后续的重渲染, React 会调用内部状态更新触发了渲染的函数组件（的组件函数），并更新虚拟 DOM 树。

{% folding yellow, React 的默认重渲染行为 %}

在默认未做任何优化的情况下，如果一个组件被重新渲染，它的所有子组件（无论子组件的 `props` 或 `state` 是否发生变化）也会被重新渲染。这意味着 React 会**递归地重新调用**这些子组件的**渲染方法**（对于类组件）或**组件函数**（对于函数组件），从而生成它们各自的虚拟 DOM，最终得到新的虚拟 DOM 树；再使用高效的 `Diffing` 算法（协调算法）**从组件状态更新引发的那个组件开始**，逐层比较新旧虚拟 DOM 树; React 以此计算出需要更新真实 DOM 的**最小差异集**； 因此，如果更新的组件在树中的位置非常高，那么默认渲染更新组件内所有嵌套组件的行为就会导致性能低下。

{% endfolding %}

### React 把更改提交到 DOM 上

&nbsp;&nbsp;&nbsp;&nbsp;渲染组件之后，React 将会修改真实 DOM。

- 对于初次渲染， React 将生成的虚拟 DOM 树转化为实际的 HTML 元素，再使用 `appendChild()` DOM API 将其创建的所有 DOM 节点放在屏幕上。
- 对于重渲染， React 将应用最少的必要操作（在渲染时计算！），以使得 DOM 与最新的渲染输出相互匹配。
