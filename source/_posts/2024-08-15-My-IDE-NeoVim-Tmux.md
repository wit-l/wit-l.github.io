---
title: My Tmux and Neovim workflow
tags:
  - IDE
  - Neovim
  - Vim
  - Tmux
  - 集成开发环境
categories:
  - 工作流
description: 本文主要介绍我的Tmux和Neovim工作流
abbrlink: c777369
date: 2024-08-15 20:59:29
sticky: 1
swiper_index: 1
cover: https://tuchuang.voooe.cn/images/2024/08/15/ailoma_1920x1080.webp
---

# 本人的工作流介绍

## <abbr title="Integrated Development Environment（集成开发环境）">IDE</abbr> -- [Neovim](#Neovim)

&nbsp;&nbsp;&nbsp;&nbsp;集成开发环境基于LazyVim预配置框架的Neovim，并根据个人爱好额外安装了一些插件，进行了个性化的配置。效果图如下：
![Neovim](https://cdn.jsdelivr.net/gh/wit-l/filebed@main/images/17237363426531723736342485.png)
&nbsp;&nbsp;&nbsp;&nbsp;Neovim配置[在这里](https://github.com/wit-l/NeovimStarter/)。其中最底部的状态栏是tmux的。

## 终端复用器 -- [Tmux](#Tmux)

&nbsp;&nbsp;&nbsp;&nbsp;使用tmux来对终端下的多个窗口（window）、会话（session）、面板（pane）进行管理。效果图如下：
![tmux](https://cdn.jsdelivr.net/gh/wit-l/filebed@main/images/17237330735801723733072788.png)
&nbsp;&nbsp;&nbsp;&nbsp;tmux使用tpm插件管理器，配置文件[在这里](https://github.com/wit-l/Dotfiles/tree/main/tmux)。

{% note info flat %}OS: WSL2(Debian12){% endnote %}

<h1 id="Neovim">Neovim</h1>

&nbsp;&nbsp;&nbsp;&nbsp;[Neovim](https://github.com/neovim/neovim)是vim的重构版本，旨在提升其可扩展性和用户体验。

1. 改进的可扩展性:

- 内置的 Lua 支持：Neovim 提供了原生的 Lua 支持，允许用户用 Lua 编写配置和插件，这比 Vimscript 更加现代、简洁和高效。
- 异步插件：Neovim 支持异步执行插件，这意味着插件可以在后台运行，不会阻塞编辑器的主线程，从而提高了性能和响应速度。

2. 用户体验:

- 更好的 UI 集成：Neovim 提供了更强的 UI 支持，包括内置终端、多窗口支持、弹出窗口等，使得创建现代化的编辑器体验更加容易。
- 内置 <abbr title="Language Server Protocol（语言服务器协议）">LSP</abbr>：Neovim 直接支持 LSP，可以为多种编程语言提供智能代码补全、跳转、重构等功能，无需依赖第三方插件。

3. 社区活跃:

- Neovim 有一个非常活跃的社区，用户和开发者不断为其贡献新功能和插件，生态系统非常丰富，适合各种编程需求。

4. 兼容性:

- Neovim 保持了与 Vim 的高度兼容性，绝大部分 Vim 插件和配置可以在 Neovim 中直接使用，同时它也引入了许多 Vim 中没有的新特性。

## LazyVim

&nbsp;&nbsp;&nbsp;&nbsp;[LazyVim](https://www.lazyvim.org/) 是一个基于 Neovim 的预配置框架，旨在提供一个开箱即用的高效开发环境。它整合了许多常用的插件和配置，方便用户快速上手并定制自己的 Neovim 配置。以下是 LazyVim 的一些关键特点：

1. 开箱即用：
   - LazyVim 提供了一个经过精心设计的默认配置，用户只需安装即可开始使用，而无需从头配置 Neovim。
   - 它预先集成了多种常用插件，涵盖代码补全、语法高亮、文件管理、Git 集成等多方面的功能。
2. 插件管理：
   - LazyVim 基于 lazy.nvim 插件管理器，提供了一个简洁高效的插件管理方式。lazy.nvim 支持懒加载，可以加快启动速度，并减少资源占用。
3. 高度可定制：
   - 虽然 LazyVim 提供了丰富的默认配置，但用户仍可以轻松地进行自定义。通过覆盖默认配置或添加自己的插件和设置，用户可以调整 LazyVim 以满足个性化需求。
4. 社区支持：
   - LazyVim 有一个活跃的社区，用户可以共享配置和经验，帮助新用户快速上手。它的持续更新和改进也得益于社区的贡献。
5. 现代化功能：
   - LazyVim 支持 Neovim 的现代化特性，如内置的 <abbr title="（语言服务器协议）">LSP</abbr>支持、Tree-sitter 语法高亮、Telescope 模糊搜索等。

<h1 id="Tmux">Tmux</h1>

&nbsp;&nbsp;&nbsp;&nbsp;<abbr title="Terminal Multiplexer">tmux</abbr>是一个终端复用器，允许用户在一个终端会话中同时管理多个终端窗口和面板。它广泛应用于远程服务器管理、开发工作流和多任务处理。关键特点：

1. 多窗口和多面板：
   - 窗口管理：tmux 允许用户在一个单一的终端会话中创建多个窗口，每个窗口都可以运行独立的 shell 或其他命令。
   - 面板管理：在同一个窗口中，用户可以将窗口分割为多个面板，每个面板也可以运行独立的 shell。这样，用户可以同时查看和操作多个任务。
2. 会话管理：
   - 持久化会话：tmux 支持创建、分离和重新连接会话。用户可以在需要时断开 tmux 会话，而后重新连接并恢复到原来的工作状态，这在远程工作时非常有用。
   - 多会话支持：用户可以同时运行多个 tmux 会话，每个会话可以包含不同的窗口和面板布局。
3. 可定制性：
   - 键绑定：tmux 提供了丰富的键绑定，用户可以通过快捷键快速切换窗口、面板和会话，还可以自定义这些键绑定以适应个人习惯。
   - 配置文件：tmux 使用一个配置文件（通常为 ~/.tmux.conf），用户可以在其中定义自定义设置、别名和脚本，来增强或改变 tmux 的行为。
4. 脚本化：
   - 自动化操作：tmux 可以通过命令行接口执行脚本化操作，例如自动创建特定布局的窗口和面板、启动特定的命令等。这对于重复性任务非常有用。
5. 远程工作：
   - 持久化连接：在远程服务器上使用 tmux，可以在网络连接中断后重新连接到相同的会话，而无需重新启动正在进行的任务。这对长时间运行的任务非常有帮助。
