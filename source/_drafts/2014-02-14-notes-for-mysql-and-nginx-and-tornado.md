---
title: nginx/mysql/tornado的一些笔记
tags:
  - python
  - mysql
  - tornado
  - nginx
permalink: notes-for-mysql-and-nginx-and-tornado
categories:
  - Linux
date: 2014-02-14 00:00:00
---

#mysql

- 登录:

        :::bash
        mysql -u root -p


- 创建gbk数据库和utf8数据库
    
        :::mysql
        GBK: create database DBNAME DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
        UTF8: CREATE DATABASE DBNAME DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;


- 创建用户，分配数据库权限

        :::mysql
        insert into mysql.user(Host,User,Password) values ('localhost','mysql',password('mysql'));
        flush privileges;
        create database mysqlDB;
        grant all privileges on mysqlDB.* to mysql@localhost identified by 'mysql';
        flush privileges;

- 导出整个数据库

        :::bash
        mysqldump -uroot -ppassword database_name > /path/to/dump.sql

- 导出数据库结构(-d)

        :::bash
        mysqldump -uroot -ppassword -d database_name > /path/to/dump.sql

- 导出表，包括结构和数据

        :::bash
        mysqldump -uroot -ppassword database_name table_name > /path/to/dump.sql

- 导出表，只有结构(-d)

        :::bash
        mysqldump -uroot -ppassword -d database_name table_name > /path/to/dump.sql

- 导出表，只有表中数据

        :::bash
        mysqldump -uroot -ppassword -t database_name table_name > /path/to/dump.sql

- 查找数据忽略大小写 case-insensitive

        :::mysql
        alter table table_name convert to character set utf8 collate utf8_general_ci;

- 查找数据区分大小写 case-sensitive

        :::mysql
        alter table table_name convert to character set utf8 collate utf8_bin;


#nginx

###下载重命名

    :::text
    location /media {
        alias   /var/www/media;
        if ($request_filename ~ ^.*?/[^/]*?_(.*?\..*?)$)
        {
            set $filename $1;
        }
        add_header Content-Disposition 'attachment; filename=$filename';
    }


#tornado


#iptables

### 过滤暴力尝试

    :::bash
    iptables -I INPUT -i eth1 -p tcp -m tcp --dport 22 -m state --state NEW -m recent --set --name DEFAULT --rsource

    iptables -I INPUT -i eth1 -p tcp -m tcp --dport 22 -m state --state NEW -m recent --update --seconds 180 --hitcount 4 --name DEFAULT --rsource -j DROP
