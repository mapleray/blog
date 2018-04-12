---
title: Nginx Download Attachment Auto Rename
tags:
  - linux
  - nginx
permalink: nginx-download-file-auto-rename
categories:
  - Programming
date: 2014-10-17 00:00:00
---

Here is the confi in nginx.conf:

	location /media {
	    alias   /var/www/media;

	    if ($request_filename ~ ^.*?/[^/]*?_(.*?\..*?)$)
	    {
	        set $filename $1;
	    }

	    add_header Content-Disposition 'attachment; filename=$filename';
	}
