---
title: deploy python app from scratch on ubuntu server
tags:
  - python
  - linux
permalink: deploy-python-app-from-scratch
categories:
  - Programming
date: 2014-06-24 00:00:00
---


## Base environment

    :::bash
    sudo apt-get install python python-dev build-essential
    sudo apt-get install python-virtualenv
    virtualenv app_virtualenv
    source app_virtualenv/bin/activate
    pip install -r requirements.txt
    pip install uwsgi

## Add a virtual host

`edit /etc/nginx/sites-available/defaults`

    :::text
    upstream app {
        server unix:///path/to/your/app.sock;
    }
    server {
        listen 80;
        charset utf-8;
        server_name your_domain;

        access_log /path/to/your/app.access.log;
        error_log /path/to/your/app.error.log;

        location / {
            include uwsgi_params;
            uwsgi_pass app;
        }

        location /static {
            alias /path/to/your/app/static;
        }

        # settings
        # 404 and 50x handler
    }



##`Flask` example as `run.py`

    :::python
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def index():
        return "<span style='color:red'>Hello World!</span>"


and the `uwsgi.ini` file:

    :::text
    [uwsgi]
    # the virtualenv (full path)
    wsgi-file       = /path/to/your/app/run.py
    # process-related settings
    callable          = app
    master          = true
    # maximum number of worker processes
    processes       = 10
    # the socket (use the full path to be safe
    socket          = /tmp/app.sock;
    #socket                 = 127.0.0.1:8080
    # ... with appropriate permissions - may be needed
    # chmod-socket    = 664
    # clear environment on exit
    vacuum          = true


## `Django` example:

    :::text
    # mysite_uwsgi.ini file
    [uwsgi]
    # Django-related settings
    # the base directory (full path)
    chdir           = /path/to/your/project
    # Django's wsgi file
    module          = project.wsgi
    # the virtualenv (full path)
    home            = /path/to/virtualenv

    # process-related settings
    # master
    master          = true
    # maximum number of worker processes
    processes       = 10
    # the socket (use the full path to be safe
    socket          = /path/to/your/project/mysite.sock
    # ... with appropriate permissions - may be needed
    # chmod-socket    = 664
    # clear environment on exit
    vacuum          = true


EOF
