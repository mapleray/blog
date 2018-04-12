---
title: django+mysql+mongodb+orm 总结
tags:
  - django
  - python
  - mongodb
permalink: django-mongdb-engine-orm-summary
categories:
  - Programming
date: 2014-04-30 00:00:00
---


给mongodb 的 objectid + django的admin 跪了，始终找不到解决方法，stackoverflow上有人提了这个问题-[问题链接](http://stackoverflow.com/questions/22376391/django-admin-and-mongoengine-objectid-instead-of-int),白白浪费几天时间，留着坑后面再来填吧


PyMySQL支持python3，不过用在 django 里面需要改一下，在 project 里面的 `__init__.py` 添加下面两行:

    :::python
    import pymysql        
    pymysql.install_as_MySQLdb()        


- ORM: 将数据关系用`类`来进行操作

- `django` 配合 `south` 进行数据库操作时，需要在项目开始前先执行`python manager.py schemamigration --initial appname`，再来同步数据库`python manager.py syncdb`，最后执行`python manager.py migrate appname`.后面数据模型变化时只需要简单执行`python manager.py schemamigration --auto appname`,`python manager.py migrate appname`即可。


- 未完待续......
