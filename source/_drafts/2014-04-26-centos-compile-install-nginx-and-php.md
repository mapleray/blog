---
title: Centos 下 nginx+php 编译安装
tags:
  - linux
  - nginx
permalink: centos-compile-install-nginx-and-php
categories:
  - Programming
date: 2014-04-26 00:00:00
---


###系统: centos 6.5 64位

- 系统安装:

    图形界面,看选项,一路下一步就可以了,最后选择`minimal`
    新建普通用户`conan:conan`进行一般操作,需要权限时在转为`root:password`

- 系统配置

    - 增加普通用户: `useradd -m -g users -G wheel conan`
    - 设定密码: `passwd conan`
    - 增加权限: `visudo`

        - 增加一行 `conan ALL=(ALL) ALL`

    - 增加nginx运行账户:

        - `groupadd www`
        - `useradd -g www www -s /bin/false`

    - 修改yum源: `sudo vi /etc/yum.repos.d/CentOS-Base.repo`
    - 生成缓存: `yum makecache`


- 编译安装nginx

        :::bash
        yum install gcc gcc-c++
        wget http://nginx.org/download/nginx-1.5.11.tar.gz
        wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.33.tar.bz2
        wget http://zlib.net/zlib-1.2.8.tar.gz
        wget http://10.0.0.168/files/81690000006558E2/de2.php.net/distributions/php-5.5.10.tar.bz2
        tar -zxvf nginx-1.5.11.tar.gz
        tar -zxvf zlib-1.2.8.tar.gz
        tar -jxvf pcre-8.33.tar.bz2

    pcre zlib:

        :::text
        ./configure --prefix=/usr/local/pcre(zlib)
        make
        make install

    nginx: 参数参考 http://wiki.nginx.org/NginxInstallOptions

        :::text
        ./configure
        --sbin-path=/usr/local/nginx/nginx
        --conf-path=/usr/local/nginx/nginx.conf
        --pid-path=/usr/local/nginx/nginx.pid
        --user=www
        --group=www
        --with-http_ssl_module
        --with-pcre=/usr/local/src/pcre
        --with-zlib=/usr/local/src/zlib

- 运行: `/usr/local/nginx/nginx`

- 测试: `curl 127.0.0.1` 显示nginx欢迎界面,安装成功。

- 编译安装php

        :::bash
        tar -jxvf php-5.5.10.tar.bz2
        ./configure --prefix=/usr/local/php
        --with-fpm-user=www
        --with-fpm-group=www
        --with-config-file-path=/etc
        --with-config-file-scan-dir=/etc/php.d
        --with-openssl--with-zlib
        --enable-bcmath
        --with-bz2
        --with-curl
        --enable-ftp
        --with-gd
        --enable-gd-native-ttf
        --with-gettext
        --with-mhash
        --enable-mbstring
        --with-mcrypt
        --enable-soap
        --enable-zip
        --without-pear
        --enable-fpm

提示缺什么就`yum`安装什么,不过`libmcrypt` 和`libmcrypt-devel`需要手动下载编译安装

    :::bash
    cd /usr/local/php       
    cp etc/php-fpm.conf.default etc/php-fpm.conf        


取消`nginx.conf`里php的`location`注释
在html文件夹里新建 `test.php`

    :::php
     <?php phpinfo(); ?>

启动`php-fpm` 和 `nginx`:

    :::text
    /usr/local/php/sbin/php-fpm
    /usr/local/nginx/nginx

访问 127.0.0.1/test.php 显示phpinfo页面,配置成功。

