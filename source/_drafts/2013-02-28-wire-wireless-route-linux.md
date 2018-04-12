---
title: 有线+无线上网配置本地路由表
tags:
  - 路由表
  - linux
  - router
permalink: wire-wireless-route-linux
categories:
  - Technology
date: 2013-02-28 00:00:00
---


最近突然发现寝室里能搜索到其它寝室的联通宽带信号，心中一阵窃喜，不过加密是肯定的。自己寝室没安宽带，加上校园网上网速度太坑爹了，心中果断有了一点点邪意~~~于是拿出利器BT5，走起！这个对于神器来说简直是小case，而且貌似密码太简单了，一下就OK了。。。。唉，罪过啊～～～不过现在校内网也有需求，我同时又连上了有线，结果上网默认走的还是有线，开始只知道这个与路由表有关，一恕之下，把这方面的知识全部恶补了，果然对于我这种网络盲来说收获颇多。

`route` 和`ip` 两个命令都能满足，不过感觉`ip`命令更强大     
######初始时:
        [mapleray@Arch ]$ route -n      
        Kernel IP routing table      
        Destination     Gateway         Genmask         Flags Metric Ref    Use Iface       
        0.0.0.0         10.143.11.1     0.0.0.0         UG    0      0        0 eth0        
        0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan0       
        10.143.11.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0        
        192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 wlan0       
                  
######最终效果：      

        [mapleray@Arch ]$ route -n      
        Kernel IP routing table         
        Destination     Gateway         Genmask         Flags Metric Ref    Use Iface       
        0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan0       
        10.0.0.0        10.143.11.1     255.0.0.0       UG    0      0        0 eth0        
        10.143.11.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0        
        192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 wlan0       
            
            
######backtrack源，教育网访问更佳。

        deb http://mirrors.ustc.edu.cn/backtrack/32 revolution main microverse non-free testing
        deb http://mirrors.ustc.edu.cn/backtrack/all revolution main microverse non-free testing
        deb http://mirrors.ustc.edu.cn/backtrack/updates revolution main microverse non-free testing
