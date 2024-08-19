---
title: 关于代理
tags:
  - 代理
  - 计算机网络
  - ping
categories:
  - 计算机基础
mathjax: true
description: 本节主要介绍使用代理时需要注意的问题
abbrlink: 5c7a2c07
sticky: 1
swiper_index: 1
date: 2024-08-19 23:03:20
cover: https://tuchuang.voooe.cn/images/2024/08/17/one_room5_1920x1080.webp
---

# 背景

&nbsp;&nbsp;&nbsp;&nbsp;使用过clash系列软件的朋友应该都用过其提供的本地代理服务器，若不了解代理服务器的原理，则只会一直开着系统代理，才能让软件的流量经过clash。在开启系统代理后，所有默认依赖系统网络配置的应用都会通过clash 代理，但其实也可以在不打开系统代理的情况下使用clash提供的服务。下面介绍在关闭系统代理的情况下如何让软件的流量经过clash的代理服务器。

# 软件配置proxy

&nbsp;&nbsp;&nbsp;&nbsp;这里以Edge/Chrome浏览器为例，当打开clash的服务并关闭系统代理后，默认情况下，软件的流量是不会经过代理的，这就需要单独为软件指定代理服务器。最直接的方式是在启动软件时指定代理，例如通过powershell打开Edge：`&'C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe' --proxy-server="http://127.0.0.1:7890"`，Chrome同理：`&'C:\Program Files\Google\Chrome\Application\chrome.exe' --proxy-server="http://127.0.0.1:7890"`。注意，这里的路径以自己的浏览器所在路径为准，此处为默认路径，后面`proxy-server`选项的值中端口号需要换成clash中设置的代理的端口号。
&nbsp;&nbsp;&nbsp;&nbsp;这样一来，就只有设置过代理的软件的流量会经过clash的代理，其他未设置过代理的软件则和原本一样直接发送出去。

{% note info flat %}
对于Edge和Chrome浏览器，还有其他更灵活的方式设置代理，可以通过插件来设置规则，让匹配的域名或网址使用某个代理或不使用代理。这里推荐本人使用的一个[插件](https://chromewebstore.google.com/detail/proxy-switchyomega-3-zero/pfnededegaaopdmhkdmcofjmoldfiped)。
{% endnote %}

# 无法代理ping命令的数据

&nbsp;&nbsp;&nbsp;&nbsp;细心的朋友可能会发现，即使时打开了系统代理，只要未开启TUN模式，无论在哪个shell中使用ping指令，都无法PING通需要科学上网的网站。

## 原因

&nbsp;&nbsp;&nbsp;&nbsp;ping发出ICMP报文，该报文在TCP/IP模型中产生于IP（网络）层，属于IP数据包的数据部分；因此其无需经过上面应用层和传输层，也就不依赖于应用层的http/https协议，甚至不依赖于传输层的TCP/UDP，而clash中的代理服务器所支持的两种代理方式：http(s)和Socks代理则分别负责代理应用层的http/https和经过传输层的任何协议数据（包括http/https/ftp/smtp/pop3等等）。ICMP报文诞生于更下层的网络层，故直接跳过了上面的传输层和应用层，其数据自然得不到clash的代理，因此，才出现上面的问题。

&nbsp;&nbsp;&nbsp;&nbsp;而开启TUN模式的情况下，clash通过在操作系统中创建一个虚拟网络适配器（类似于虚拟网卡）来捕获并转发真实网络适配器的所有流量。这相当于在数据链路层（虚拟适配器）和网络层 上进行了一次流量重定向，使得系统中所有应用的流量都可以透明地通过clash 代理进行转发和处理。这种机制让TUN 模式可以代理更多类型的流量，包括无法通过传统应用层代理（如http/https 代理）捕获的非http流量。

## 探测网络连通性的工具

&nbsp;&nbsp;&nbsp;&nbsp;如果需要通过代理来探测网络连通性，通常不会使用 `ping`，而是使用基于 TCP/UDP 的工具，如 `telnet`、`curl`或者专门的网络探测工具，它们可以通过应用层或传输层协议的流量来测试代理后的连接是否正常。

{% note info flat %}
注：clash中的混合代理并非一种新的代理协议，而是一种智能切换代理的方式，使用该代理方式时，clash会智能判断流量的类型（HTTP、HTTPS 或 SOCKS），并根据需要进行相应的处理,使用这种代理的端口更加灵活;curl常用于下载文件，若访问的网站进行了重定向，则还需要加`-L`选项才能正确访问到重定向后的网站，从而获取到数据。
{% endnote %}
