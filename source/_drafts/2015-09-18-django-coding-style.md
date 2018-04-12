---
title: Django 代码规范
tags:
  - python
  - django
categories:
  - Python
permalink: django-coding-style
date: 2015-09-18 01:20:00
---


最近在看关于 Django 的一些资料，简要整理在此。


### 代码规范

> - Avoid abbreviating variable names.
> - Write out your function argument names.
> - Document your classes and methods.
> - Comment your code.
> - Refactor repeated lines of code into reusable functions or methods.
> - Keep functions and methods short. A good rule of thumb is that scrolling should not be necessary to read an entire function or method.


### Python Import 顺序

> - Standard library imports.
> - Related third-party imports.
> - Local application or library specific imports


### Django Import 顺序

> - Standard library imports.
> - Imports from core Django.
> - Imports from third-party apps including those unrelated to Django.
> - Imports from the apps that you created as part of your Django project.


Example:

    # Stdlib imports
    from __future__ import absolute_import
    from math import sqrt
    from os.path import abspath

    # Core Django imports
    from django.db import models
    from django.utils.translation import ugettext_lazy as _

    # Third-party app imports
    from django_extensions.db.models import TimeStampedModel

    # Imports from your apps
    from splits.models import BananaSplit


### Get into the habit of using explicit relative imports.

