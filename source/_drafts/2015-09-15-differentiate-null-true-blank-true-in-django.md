---
title: Django 数据模型中 null=True 和 blank=True 的区别
tags:
  - Django
permalink: differentiate-null-true-blank-true-in-django
categories:
  - programming
date: 2015-09-15 00:00:00
---


详细可参考 [stackoverflow](http://stackoverflow.com/questions/8609192/differentiate-null-true-blank-true-in-django)

> `null=True` sets `NULL` (versus `NOT NULL`) on the column in your DB. Blank values for Django field types such as `DateTimeField` or `ForeignKey` will be stored as `NULL` in the DB.


> `blank=True` determines whether the field will be required in forms. This includes the admin and your own custom forms. If `blank=True` then the field will not be required, whethereas if it's `False` the field cannot be blank.


简单的说就是:

- `null` 是针对数据库而言，如果 `null=True`, 表示数据库的该字段可以为空。
- `blank` 是针对表单的，如果 `blank=True`, 表示表单填写该字段的时候可以不填。



