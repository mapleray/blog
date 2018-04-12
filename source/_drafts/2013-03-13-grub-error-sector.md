---
title: 折腾黑苹果，修复grub
tags:
  - y470
  - linux
permalink: grub-error-sector
categories:
  - Technology
date: 2013-03-13 00:00:00
---

####不正常的心态
  不知道怎么想的，这几天突发奇想，想在本本上装一下黑苹果。其实在我刚入手笔记本的时候最先折腾的就是黑苹果了，最可笑的是当时对Unix根本没概念，甚至连什么是终端都不知道，竟然有勇气折腾下去0_0。。。。按着网上的教程，竟一步到位，安装好了，没出现其他网友出现的问题，估计是RP爆发吧，当时记得除了无线网卡驱动外，其他基本完美，不过毕竟不是原生苹果硬件，体验的感觉始终不怎么好，后面逐渐了解了Linux以及相关的，转战Ubuntu了，不过没在它上找到我对linux的感觉，又投奔Archlinux了。        
 不过最近逛黑苹果论坛，发现又支持独显了，网卡问题也完美解决，这又点燃了我那装13的心。结果。。。。。。坑！      
        
  现在再回黑苹果，已是轻车熟路了。不过win下对mac盘的识别软件由 `macdriver` 换成了 `HFS for Windows` ，悲剧来了。安装后重启，grub失效了，提示不能识别硬盘格式，系统启动不了了，果断PE修复之，当然结果是archlinux暂时没了，想着黑苹果的事，也就先没管了。突然发现黑苹果安装没压力啊～～～后面也把各种驱动安好了，体验更加好了。不过纠结的我内心又纠结了。安黑苹果系统有用吗？折腾这个你能学会什么？典型213青年还想学别人装13？。。。然后一怒之下，格式化了苹果盘。还是滚回我的arch吧. 唉，我都不知道我是怎么想的。。。
        
####Grub问题
 现在又回到开始状态了，白折腾了，不过现在要解决的是grub引导问题。进入liveCD,挂载，但是之后我没注意，用的是 `chroot` 命令 ,结果出现 `Path /boot/grub is not readable on GRUB boot ` ，后面发现应该用 `arch-chroot` 这个命令，暂时还不知道这两个有什么区别。一个问题解决了，又出现了一个。当我执行 `grub-install` 时，出现 `Sector 32 is already in use by the program 'FleNet'` ,估计这个应该和用PE修复引导有关。下面是解决办法：

    :::bash
    dd if=/dev/sda of=/tmp/mbr.img bs=512 count=63      
    dd if=/dev/zero of=/dev/sda bs=512 count=62 seek=1      

现在再执行 `grub-install` 一切顺利。终于又见了我的arch了。

####后记
 由此总结了一点：我太傻x了！

