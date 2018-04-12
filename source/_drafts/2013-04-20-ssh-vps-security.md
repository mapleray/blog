---
title: vps折腾之SSH 设置
tags:
  - linux
  - ssh
permalink: ssh-vps-security
categories:
  - Technology
date: 2013-04-20 00:00:00
---


####管理好root帐号以及登录权限!!
        
增加普通用户：

    :::bash
    useradd -m -G wheel $USER

修改SSH 端口：

    :::text
    vi /etc/ssh/sshd_config
    vi /etc/ssh/ssh_config
    增加 Port 4444

禁用root登录：

    :::text
    vi /etc/ssh/sshd_config
    修改 `PermitRootLogin yes --> PermitRootLogin no

ssh-keygen 生成证书：

    :::text
    id_rsa.pub --> authorized_keys
    再使用`chmod`更改权限

禁用密码登录：

    :::text
    PasswordAuthentication no

重启sshd服务：

    :::text
    service sshd restart

####配置iptables:

清楚已有规则：

    :::bash
    iptables -F
    iptables -X
    iptables -Z

允许本地回环：

    :::bash
    iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT

允许已建立的或相关的通信：

    :::bash
    iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

允许本机向外访问：

    :::bash
    iptables -A OUTPUT -j ACCEPT

增加端口：

    :::bash
    iptables -A INPUT -p tcp --dport 4444 -j ACCEPT
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT

禁止其他未允许的规则访问：

    :::bash
    iptables -A INPUT -j REJECT
    iptables -A FORWARD -j REJECT

若无法ping，还需要添加如下命令：

    :::bash
    iptables -L -n --line-numbers

将`INPUT`里面的`reject-with icmp-port-unreachable`那一条删除,如果要删除的`INPUT`里面的`reject-with icmp-port-unreachable`这条规则的序号为8，则执行：`iptables -D INPUT 8`

查看已添加的iptables规则：

    :::bash
    iptables -L -n

规则保存及开机启动

    :::bash
    chkconfig --level 345 iptables on
    service iptables save

最后重启即可

