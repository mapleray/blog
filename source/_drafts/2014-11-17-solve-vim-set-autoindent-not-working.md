---
title: Vim autoindent not working
tags:
  - vim
permalink: solve-vim-set-autoindent-not-working
categories:
  - Linux
date: 2014-11-17 00:00:00
---

Problem: VIM's autoindent setting not works

Solution: Change the order in vimrc file. Set `set paste` before `set autoindent` and `set cindent`.

P.S.: see vim doc `:h autoindent`, find that

>   The 'autoindent' option is **reset** when the 'paste' option is set.
    {small difference from Vi: After the indent is deleted when typing
    <Esc> or <CR>, the cursor position when moving up or down is after the
    deleted indent; Vi puts the cursor somewhere in the deleted indent}.

So when having `set paste` in vimrc file, `set paste` must exist firstly.

See `:h paste` to get more reset option.

