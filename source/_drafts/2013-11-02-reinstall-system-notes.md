---
title: 重装系统的一些记录
tags:
  - linux
  - archlinux
  - xfce
permalink: reinstall-system-notes
categories:
  - Technology
date: 2013-11-02 00:00:00
---

#### 系统：Archlinux (x86_64)
#### 桌面环境：xfce4

1. thunar 自动挂载U盘：
                
        pacman -S udisks gvfs gvfs-afc thunar-volman
        yaourt -S uevt
                
2. python-virtualenv 环境配置

> 详见 [python virtualenv笔记](http://sillymind.me/2013/05/python-virtualenv/)

3. 安装 **mit-scheme**
        
        vim ~/.emacs
        (setq show-paren-delay 0
            show-paren-style 'parentheses')
        (show-paren-mode 1)
        (setq scheme-program-name "mit-scheme") 

4. y470 linux下 bumblebee 解决双显卡问题
> 详见 [Y470/570系列linux下双显卡bumblebee完美解决攻略](http://sillymind.me/2012/12/arch-linux-bumblebee/)

5. vim, emacs, git 等软件配置
> 配置全部在github上， clone 下来稍微改改就可以用了，

6. 其他
> 各种主题基本都用的 **[solarized](https://github.com/altercation/solarized)**

7. 常用    
[x] [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)    
[x] [powerline-shell](https://github.com/milkbikis/powerline-shell)    
[x] [powerline-fonts](https://github.com/Lokaltog/powerline-fonts)    
[x] [vim-powerline](https://github.com/Lokaltog/vim-powerline)    
        
8. 未完待续......
              
              
P.S. 还是抵挡不了诱惑，又来捣鼓了，时间都浪费在了配置系统上，而自己却什么都没学到，该反思了。

