---
title: Archlinux下用Sphinx生成中文pdf
tags:
  - python
  - sphinx
permalink: Archlinux-sphinx-to-pdf-Chinese
categories:
  - Technology
date: 2014-03-19 00:00:00
---


GitHub上很多文档都是用 Sphinx 制作的，有了文档的源码，翻译好再用 Sphinx 生成中文版的就可以,方便阅读。不过在生成 中文 pdf 文档的过程却遇到很多问题。虽然 Sphinx 是用 Python 写的，Python 对 UTF8 编码的支持毋庸置疑，但生成 pdf 的过程是由 LaTeX 套件完成。问题在于 LaTeX 套件从开发之日起就没有考虑过支持如中文这样使用 Unicode 字符的语言。

google一圈发现网上很多文章还是不能解决 Archlinux 下生成中文 pdf 问题。然后自己研究最后还是把问题解决了。

    :::bash
    sudo pacman -S texlive-most texlive-langextra texlive-langcjk

本文以 [Requests](https://github.com/requests/requests-docs-cn) 为例说明一下。

首先 clone 到本地然后 `make latex` ，这会在 `requests-docs-cn/docs/_build/latex` 产生一堆latex的相关文件，修改其中的 `Requests.tex` 。

在 `\begin{document}` 一行之前添加一行r: `\usepackage{CJKutf8}`
在 `\begin{document}` 一行之后添加一行: `\begin{CJK}{UTF8}{gkai}`

然后在`Requests.tex`最后的 `\end{document}` 一行之前添加一行r:： `\end{CJK}`

然后命令行执行 `pdflatex Requests.tex` 就可以了。

其实这个方法是[youngsterxyf](http://youngsterxyf.github.io)以前教我的，不过他当时用的是ubuntu，而我用Archlinux却没成功，后面发现少了CJK包。

