---
title: Y470/570系列linux下双显卡bumblebee完美解决攻略(已过时)
tags:
  - linux
  - y470
  - bumblebee
  - 双显卡
permalink: arch-linux-bumblebee
categories:
  - Technology
date: 2012-12-14 00:00:00
---

蛋疼的从众心里，从原本不怎么费力气就用的舒舒服服的`ubuntu`转到了`archlinux`
接下来便是惨无人道的无尽的自找的折磨......

本本安装的是`archlinux-2012-12-01-dual`，机子是`Y470`，一块集显，一块`Nvidia GT 550M`。

准备工作：一个可用的网络，添加一个速度的很好的源，取消`/ect/pacman.conf` 中 `[multilib]` 的注释 （后面会用到），安装`yaourt`, `cmake`,  `gcc`等一些基本工具。

archlinux的wiki基本很全面了，建议安装之前看一遍，由于现在的arch默认的是`systemd`,后面的步骤就有点变化了。文章中会详细说明的。(wiki已经更新了)

1. 安装lib32-virtualgl 这是64 位系统上运行 32 位应用程序必须要的

    :::bash
    yaourt lib32-virtualgl    // 后面会有很多依赖，一直安装就行

2. 安装bumblebee

    :::bash
    yaourt bumblebee nvidia-utils-bumblebee
    //*注意看wiki上的警告*
    yaourt nvidia-bumblebee     //bumblebee定制的nvidia驱动

3. 安装开源的nvidia驱动

    :::bash
    pacman -S xf86-video-nouveau nouveau-dri mesa

4. 添加相关用户到Bumblebee组

    :::bash
    usermod -a -G bumblebee $USER   //$USER 是要添加的用户登录名称。 之后注销，并重新登录，以应用组变更。

5. 添加自动启动

由于改成systemd 了， 不用修改/etc/rc.conf  而是换一种方法

    :::bash
    systemctl enable bumblebeed.service

6. 使用后自动关闭nvidia显卡

    :::bash
    yaourt bbswitch

设置`/etc/bumblebee/bumblebee.conf` 中驱动一节的`PMMethod` 为 `bbswitch`

    :::text
    /etc/bumblebee/bumblebee.conf
    [bumblebeed]
    KeepUnusedXServer=false
    ...
    [driver-nvidia]
    PMMethod=bbswitch
    ...
    [driver-nouveau]
    PMMethod=bbswitch
    ...

到此，一般的双显卡电脑就能完美使用了
比如：*独显打开火狐*:  `optirun firefox`

不过对于y470/y570还必须hack
安装hack

    :::bash
    yaourt hack-lenovo

将`acpi-handle-hack`添加到启动modules,以便在开机的时候，让hack过的acpi接管内核的apci
用systemd开机加载模块的方式，相当于在/etc/modules-load.d/下创建一个模块配置，里面只需加上模块名字就行了。

    :::bash
    vim /etc/modules-load.d/acpi-handle-hack.conf
    #Load acpi-handle-hack at boot
    acpi-handle-hack

之后重启,输入以下命令查看

    :::bash
    lspci |grep -i vga

应该会出现

    :::text
    00:02.0 VGA compatible controller: Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller (rev 09)
    01:00.0 VGA compatible controller: NVIDIA Corporation Device 0df6 (rev ff)

看到Nvidia卡的信息的末尾是`rev ff`，表示已经`disable`了
`optirun glxspheres` 会出现3D测试画面  至此，在Lenovo Ideapad Y470/Y570上成功地解决双显卡问题。


后记：╮(╯▽╰)╭，从安装到配置基本满意，这个archlinux花了我整整一周，爱折腾，没办法~~~~

PS:   鸟语不行伤不起啊   重装系统安装这个bumblebee把acpi打成了apci～～～f4k

浪费了我两天时间找不成功的原因，恍然发现是打错字了，泪奔了！！！！update:2013-1-4

PPS: 目测已经无效果了，看wiki吧 update: 2014
