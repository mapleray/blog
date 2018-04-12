---
title: Archlinux日志分析
tags:
  - linux
permalink: check-boot-arch-gnome
categories:
  - Technology
date: 2013-06-16 00:00:00
---


持续了两周的arch开机到显示gdm登录界面至少3分钟的情况，实在无法忍受了才有了这篇小记。    
首先感谢一下[仙子](http://lilydjwg.is-programmer.com/)，过程都是仙子指导的，最后的问题也是仙子发现的～～    
起因就是开机过程至少3分钟，最开始以为一两次属于正常情况，所以没管，后面一周都是这个情况，因为是升级了Gnome3.8后才出现这种情况的，就大致去*Wiki*查了一下，*Wiki*上说的最可能的原因就是开机时启动了声音什么的，反正该禁用的，不该禁用的全部禁用了，但是没效果，启动还是等得蛋疼。google也无果后就没管了。丫的，忍耐是用限度的啊～，一直这么慢真心受不了，便想到了启动日志，结果在`/var/log`里看了半天没找到我想要的东西，便想仙子求助，告知查看`/var/log/everything.log`，悲剧的是没这个日志文件，无奈又求助，发现要安装`syslog-ng`，之后重启依然没有，那是没用`systemctl enable`......查看`everything.log`的信息，前面部分是估计是内存写入什么的看不懂，我也没怎么认真看，我想找的就是看哪里时间执行的更长，结果粗略的看了后发现*NetworkManager*用了两分钟，怀疑是这个问题，反正是一通的禁用卸载这类的方式，但是还是没解决问题。后面仙子又建议查看`bootchart`,还好`systemd`已经有了`systemd-bootchart`了，只需要在启动的时候加上内核参数`init=/usr/lib/systemd/systemd-bootchart`，好不容易搞出`bootchart.svg`，结果这个图上只有systemd的启动时间，其它的都没有。没办法，只好把日志交给仙子帮忙分析了。     
仙子果然给人惊喜～～～（看看日志时间，半夜麻烦仙子滴～～汗）

    :::text
    Jun 16 00:00:15 Y470 systemd[1]: Job dev-disk-by\x2duuid-1a46e4c8\x2d6667\x2d49e9\x2d968f\x2d9a11be679514.device/start timed out.
    Jun 16 00:00:15 Y470 systemd[1]: Timed out waiting for device dev-disk-by\x2duuid-1a46e4c8\x2d6667\x2d49e9\x2d968f\x2d9a11be679514.device.
    Jun 16 00:00:15 Y470 systemd[1]: Dependency failed for /dev/disk/by-uuid/1a46e4c8-6667-49e9-968f-9a11be679514.

意思就是找不到`uuid=xxxx`的分区，查了一下`fstab`中指向的是`swap`分区，突然想起了之前我安装另一个linux系统的时候直接用的是同一个`swap`分区，自己傻逼了。后面查了一下这个`uuid`(详见[arch-wiki](https://wiki.archlinux.org/index.php/Persistent_block_device_naming#by-uuid)or[wikipedia](http://en.wikipedia.org/wiki/UUID))，反正现在的情况就是两个linux系统下`swap`分区的`uuid`不一样导致的。终于找到原因了。直接`ls -l /dev/disk/by-uuid/`找到相应的便解决了。不过现在希望的是两个linux系统下都能挂载swap分区，看wiki上感觉是通过写`<label>`能达到这个效果。    
PS：其实这个是我装archlinux以来第一次看日志，上一次都是一年前用ubuntu的时候看过一次，学艺不精，还需要多多学习啊。 
