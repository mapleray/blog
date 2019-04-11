---
title: Mac 下配置 mit-scheme 环境
tags:
  - sicp
  - scheme
categories:
  - programming
permalink: mac-mit-scheme-with-emacs-and-sublime-text
date: 2016-02-17 00:00:00
---


### 安装 mit-scheme

```
brew install mit-scheme
```

### 运行

1. 命令行输入 `mit-scheme` 进入交互模式
2. 切换工作目录

		(cd "~/workspaces")

3. 加载 scheme 源文件

		(load "hello.scm")

4. 退出交互环境

		(quit)

### Emacs 中集成 mit-scheme

- 编辑 `~/.emacs` 文件，加入下列语句:

		;;;Always do syntax
		(global-font-lock-mode 1)

		(setq show-paren-delay 0
      		show-paren-style 'parentheses)
		(show-paren-mode 1)
		;;; 若直接使用 mit-scheme 后在 emacs 里执行 run-scheme 报错，则最好使用 mit-scheme 的绝对路径
		(setq scheme-program-name "/usr/local/bin/mit-scheme")

- 打开 emacs ，使用命令 `M-x run-scheme` 即可


### sublime-text 中集成 mit-scheme

1. 安装 `Scheme` 和 `SublimeREPL` 两个插件
2. 避免出现 *无法加载Scheme.tmLanguage* 需手动安装[sublime-scheme-syntax](https://github.com/masondesu/sublime-scheme-syntax), 放到 `Packages` 目录下即可

		cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
		git clone git@github.com:masondesu/sublime-scheme-syntax.git

3. Sublime左右显示两个Layout。

		View > Layout > Columns:2

4. 运行 scheme 交互

		Tools > SublimeREPL > Scheme

p.s.: 看来 sublime 的 `build` 文档，还是没法实现直接 `command + b` 来直接运行当前的 scheme 源文件

