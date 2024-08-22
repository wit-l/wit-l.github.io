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
sticky: 1
swiper_index: 2
---

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

&nbsp;&nbsp;&nbsp;&nbsp;useState有两种原因会导致组件的渲染:

- 组件的初次渲染。
- 组件（或者其祖先之一）的[状态发生了改变](#)

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
      alt="'Floralis Genérica' by Eduardo Catalano: a gigantic metallic flower sculpture with reflective petals"
    />
  );
}
```

<!-- endtab -->

{% endtabs %}
