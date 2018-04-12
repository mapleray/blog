---
title: python virtualenv笔记
tags:
  - python
permalink: python-virtualenv
categories:
  - Technology
date: 2013-05-30 00:00:00
---


##python-virtualenv 安装

####OS: Archlinux

####软件包安装

    :::bash
    sudo pacman -S python2-virtualenv
    sudo pacman -S python-virtualenv
    yaourt -S python-virtualenvwrapper

####添加环境变量至 `~/.bashrc`

    :::bash
    export WORKON_HOME=~/virtualenvs
    export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
    source /usr/bin/virtualenvwrapper.sh

####基本使用
- 创建虚拟环境:

        :::text
        mkvirtualenv [-a project_path] [-i package] [-r requirements_file] [virtualenv options] ENVNAME

    例: `mkvirtualenv heroku --python=/usr/bin/python2`

- 列出已创建的环境：

        :::text
        lsvirtualenv [-b] [-l] [-h]
            -b  Brief mode, disables verbose output.
            -l  Long mode, enables verbose output. Default.
            -h  Print the help for lsvirtualenv.

- 删除已存在的环境:

        :::bash
        rmvirtualenv ENVNAME  #需要先禁用需删除的环境 deactivate

- 列出或更改工作环境:

        :::bash
        workon [environment_name]

- 禁用当前环境:

        :::bash
        deactivate

