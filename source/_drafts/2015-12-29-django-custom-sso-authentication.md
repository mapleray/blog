---
title: Django Custom SSO Authentication
tags:
  - Python
  - Django
categories:
  - Programming
permalink: django-custom-sso-authentication
date: 2015-12-29 00:00:00
---


Django 自定义 SSO 验证
=========

### 原理
由于项目需要能直接登入员工的帐号，集团申请了一个验证帐号的接口，需要集成在`Django` 开发的项目中。`Django` 自定义认证方式也很方便，官方文档 [Customizing authentication in Django](https://docs.djangoproject.com/en/dev/topics/auth/customizing/) 也很清除，大致的方法就是自己写一个验证的 `backend`，需要实现 `get_user(user_id)` 和 `authenticate(**credentials)` 两个方法。每次员工帐号登录走公司的认证系统，认证成功后在项目的`USER`中存一个员工的实例即可，密码什么的不必要存。

### 代码
下面的代码可以自定义验证成功后对 `user` 的一些操作，比如添加权限什么的。

*注: `check_user` 是自己写的验证员工的函数，可以根据需要改成其他的*

```python
class SSOBackend(object):
    '''
    Authenticate staff.
    '''
    def authenticate(self, username=None, password=None):
        is_checked, user_info = check_user(username, password)
        if is_checked:
            return self.get_or_create_user(username, password, user_info)
        else:
            return None

    def get_or_create_user(self, username, password, user_info):
        UserModel = get_user_model()
        try:
            user = UserModel._default_manager.get(email=user_info.get('email'))
        except UserModel.DoesNotExist:
            user = UserModel()
            
            /.......
            your code for user
            ......../
            user.set_unusable_password('not_a_real_password')
            user.is_staff = True
            user.save()
       return user

    def get_user(self, user_id):
        UserModel = get_user_model()
        try:
            return UserModel._default_manager.get(pk=user_id)
        except:
            return None

```

然后在项目的 `settings.py` 里面添加自己写的 `backend` 即可


