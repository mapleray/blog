---
title: 科学上网的一些原理
tags:
  - proxy
categories:
  - Network
permalink: the-theory-of-across-gfw
date: 2016-01-24 00:00:00
---

### GFW 是什么
简单说，GFW 部署在天朝互联网的国际出口，只要你的数据不是加密传输，它都能知道你在干什么，然后针对一系列列表做过滤，然后你就会遇到[链接被重置](https://zh.wikipedia.org/zh-sg/%E8%BF%9E%E6%8E%A5%E9%87%8D%E7%BD%AE)。

### 科学的姿势
只有一点，绕过 GFW 的检测，就能正常的上网了。

第一种方式，只需要一台能正常访问的国外 vps。
![one-vps](/images/one-vps.png)
上网设备在上网时，先加密数据包，然后发送到国外 vps，再由国外 vps请求数据，最终实现无阻上网。

第二种方式，需要国内一台 vps 和国外一台 vps。
![two-vps](/images/two-vps.png)
国内 vps 与国外 vps 通信是加密的，这种方式通过国内 vps 分流，速度也更快，曲径的大致工作原理就是这样，下一篇重点讲如何实际部署已经 iOS 上配置 APN。

### 延伸阅读
1. [闲谈VPN、APN和曲径](http://getqujing.tumblr.com/post/82246191116/%E9%97%B2%E8%B0%88vpnapn%E5%92%8C%E6%9B%B2%E5%BE%84)
2. [写给非专业人士看的 Shadowsocks 简介](http://vc2tea.com/whats-shadowsocks/)

