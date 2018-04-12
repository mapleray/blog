---
title: archlinux下awesome折腾记
tags:
  - awesome
  - linux
permalink: awesome-configuration
categories:
  - Technology
date: 2013-03-15 00:00:00
---


在观望许久后，入手了个21.5的显示器。于是果断奔向awesome，毕竟多屏下awesome更加霸气。 
####Awesome
关于Awesome，详细见[Awesome wiki](https://wiki.archlinux.org/index.php/Awesome)

####awesome 配置笔记
#####应用程序菜单 
awesome 配置还算简单，网上一大堆，再参考下wiki基本就能满意。不过在配置应用程序菜单时，我开始是一个一个的自己添加，后面想想应该有脚本一次生成之类的，果然在[依云](http://lilydjwg.is-programmer.com/2011/9/7/generate-application-menu-for-awesome.29327.html)博客中找到了。先安装`archlinux-xdg-menu` 软件包

    :::bash
    xdg_menu --format awesome --root-menu /etc/xdg/menus/arch-applications.menu > ~/.config/awesome/menu.lua

然后修改 `rc.lua` ,把生成的菜单加上就可以了:

        require("menu")
        mymainmenu = awful.menu({ items = { { "Awesome", myawesomemenu, beautiful.awesome_icon },    
          -- ...    
          { "应用程序 (&A)", xdgmenu },    
          -- ...
		  
#####多显示器配置

    :::bash
    xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
    xrandr --addmode VGA1 "1920x1080_60.00"
    xrandr --output LVDS1 --mode "1366x768"
    xrandr --output VGA1 --mode "1920x1080_60.00" --right-of LVDS1


##### awesome 平铺窗口无空隙
将主题里的 `border_width` 值改为 **0** 即可

##### awesome 里使用gtk主题
下载 `lxappearance` ，在下载自己喜欢的gtk主题就行了，这下就能完全扔掉 `gnome-deamon` 什马的了.

###### last update: 2013-09-22
