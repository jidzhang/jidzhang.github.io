---
title: "Git记住密码"
date: 2013-10-15
draft: false
slug: "git-password-caching"
categories:
  - "Geek"
---

每次 `git pull/push` 的时候都要输入用户名和密码是很痛苦的，但是可以通过修改git配置来解决这个问题，并且不难。

* 首先，要求git版本要比1.7.10新，这个通常没问题。

* For [**Linux**](https://help.github.com/articles/set-up-git#platform-linux):
```bash
$ git config --global credential.helper cache
# 默认缓存密码15分钟，可以改得更长, 比如1小时
$ git config --global credential.helper 'cache --timeout=3600'
```

* For [**Windows**](http://gitcredentialstore.codeplex.com/) :

        download & install git-credential-winstore.exe

**DONE!**
