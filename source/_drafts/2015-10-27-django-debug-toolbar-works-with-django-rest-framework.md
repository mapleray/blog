---
title: Django Debug Toolbar Works With Django Rest Framework
tags:
  - django
  - python
categories:
  - Python
permalink: django-debug-toolbar-works-with-django-rest-framework
date: 2015-10-27 00:00:00
---


正常情况下，`Django` 配置好 `django-debug-toolbar` 后浏览界面应该能正常显示各类信息的，但是由于使用了 `django-rest-framework` 后，返回的是 `json` 字符串，所以 `django-debug-toolbar` 的信息一直没法显示。谷歌后发现是 `django-debug-toolbar` 只有当返回结果为`html` 内容时才正常工作，故可以加一个 中间件 `middleware` 来处理，当然 [stackoverflow](http://stackoverflow.com/questions/14618203/how-to-use-django-debug-toolbar-for-django-tastypie
) 已经有现成的了：


```python
# settings-dev.py
from django.http import HttpResponse
import json   

MIDDLEWARE_CLASSES += (
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    'yourapp.settings-dev.NonHtmlDebugToolbarMiddleware',
)

class NonHtmlDebugToolbarMiddleware(object):
    """
    The Django Debug Toolbar usually only works for views that return HTML.
    This middleware wraps any non-HTML response in HTML if the request
    has a 'debug' query parameter (e.g. http://localhost/foo?debug)
    Special handling for json (pretty printing) and
    binary data (only show data length)
    """

    @staticmethod
    def process_response(request, response):
        if request.GET.get('debug') == '':
            if response['Content-Type'] == 'application/octet-stream':
                new_content = '<html><body>Binary Data, ' \
                    'Length: {}</body></html>'.format(len(response.content))
                response = HttpResponse(new_content)
            elif response['Content-Type'] != 'text/html':
                content = response.content
                try:
                    json_ = json.loads(content)
                    content = json.dumps(json_, sort_keys=True, indent=2)
                except ValueError:
                    pass
                response = HttpResponse('<html><body><pre>{}'
                                        '</pre></body></html>'.format(content))

        return response
```
***注: 由于使用的是`Django 1.7`，所以在`MIDDLEWARE_CLASSES`里面应该写成`yourapp.settings-dev.NonHtmlDebugToolbarMiddleware`***



