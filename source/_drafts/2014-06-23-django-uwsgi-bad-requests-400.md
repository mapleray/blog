---
title: django + UWSGI 出现 Bad Request (400) Error 解决办法
tags:
  - python
  - django
permalink: django-uwsgi-bad-requests-400
categories:
  - Programming
date: 2014-06-23 00:00:00
---


When I deploy django app with uwsgi in my EC2, `400 bad requests` error occurs.So google it and find the solution in django documents.

> When DEBUG is True or when running tests, host validation is disabled; any host will be accepted. `Thus it’s usually only necessary to set it in production`.


More details see it [here](https://docs.djangoproject.com/en/1.6/ref/settings/#std%3asetting-ALLOWED_HOSTS)

So, solved by follows:

    :::python
    ALLOWED_HOSTS = [
        '.example.com', # Allow domain and subdomains
        '.example.com.', # Also allow FQDN and subdomains
    ]


EOF
