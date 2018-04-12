---
title: wamp集成环境更改mysql数据库密码
tags:
  - mysql
  - wamp
permalink: mysql-password-wamp
categories:
  - Technology
date: 2013-04-23 00:00:00
---


wamp环境初始的mysql用户名为root,密码为空。
 修改初始密码步骤：

1. 首先要在mysql数据库控制台中更改`root`用户的密码

        :::mysql
        update user set password=password("new_pass") where user="root"`;
        mysqladmin -u root -p 旧密码 password 新密码;  //只针对mysql管理员

2.  此时更改完mysql数据库的密码，但是此时是进不去phpmyadmin，此时会提示，数据库密码与phpmyadmin配置文件中的密码不一致，因为phpmyadmin配置文件中有记录mysql数据库root的密码，所以还要改phpmyadmin配置文件。     
        
3. 在phpmyadmin目录下找到`config.inc.php`文件，更改

        :::text
        `$cfg['Servers'][$i]['password'] = '新密码'

4. 此次修改完，如果你还进不去phpmyadmin，可能是wamp的缓存的问题，默认读取的还是缓存中的信息，要删除wamp目录下的temp文件夹（该文件夹为缓存）。此时再进入phpmyadmin就可以成功啦。修改密码成功。

