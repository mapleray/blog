---
title: 同时推送到多个git库
tags:
  - git
permalink: push-to-multiple-git-repos
categories:
  - Programming
date: 2014-01-16 00:00:00
---


Git的一个小技巧，自己练习代码需要，将代码部署到heroku的同时也将代码推送到Github上。很简单，只须修改相应库的  `.git/config` ：        

    :::text
    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
    [remote "origin"]
        url = git@heroku.com:pydjango.git
        url = git@github.com:mapleray/heroku.git
    [remote "github"]
        url = git@github.com:mapleray/heroku.git
        fetch = +refs/heads/*:refs/remotes/github/*
    [remote "heroku"]
        url = git@heroku.com:pydjango.git
        fetch = +refs/heads/*:refs/remotes/heroku/*
        
自己设置成上面内容后可以直接 `git push origin master`,当然也可以只推送到某个分支
