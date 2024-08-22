---
title: react note
tags:
  - react
  - web
categories:
  - React框架
description: 本文主要记录React的学习笔记
abbrlink: 1e564abe
date: 2024-08-22 21:40:35
---

## 组件的记忆

- 局部变量无法在多次渲染中持久保存
- 更改局部变量不会触发渲染

&nbsp;&nbsp;&nbsp;&nbsp;useState Hook 提供了这两个功能：

- State 变量 用于保存渲染间的数据。
- State setter 函数 更新变量并触发 React 再次渲染组件。

## 总结

- 当一个组件需要在多次渲染间“记住”某些信息时使用 state 变量。
- State 变量是通过调用 useState Hook 来声明的。
- Hook 是以 use 开头的特殊函数。它们能让你 “hook” 到像 state 这样的 React 特性中。
- Hook 可能会让你想起 import：它们需要在非条件语句中调用。调用 Hook 时，包括 useState，仅在组件或另一个 Hook 的顶层被调用才有效。
- useState Hook 返回一对值：当前 state 和更新它的函数。
- 你可以拥有多个 state 变量。在内部，React 按顺序匹配它们。
- State 是组件私有的。如果你在两个地方渲染它，则每个副本都有独属于自己的 state。
