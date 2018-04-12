---
title: Install Rz Sz With Iterm2 On Mac Osx
tags:
  - osx
  - iterm2
categories:
  - Programming
permalink: install-rz-sz-with-iterm2-on-mac-osx
date: 2016-01-13 00:00:00
---


需要从服务器拷贝一点数据到本地，但是今天开发机挂了，又是通过堡垒机连到服务器的，之前只知道到`scp`的存在，今天没法用了，后面搜索才知道原来还有 `rz`和`sz`的存在。

简单说 `rz`和`sz` 是通过 `ZModem` 协议在远程服务器和终端机器间实现上传下载文件的.

Mac 上安装步骤简要如下：

1. 安装`lrzsz`

    `brew install lrzsz`

2. 安装 iterm2, 可去官网直接下载
3. 安装 `iterm2-zmodem`，具体安装步骤见[iterm2-zmodem](https://github.com/mmastrac/iterm2-zmodem)

