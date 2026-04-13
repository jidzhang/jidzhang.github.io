---
title: "Linux/BSD/Unix 更改用户的登录shell"
date: 2015-04-15
draft: false
slug: "linux-change-shell"
categories:
  - "Linux"
---


可以通过一条命令更改用户的shell类型，如下：

（1）确保系统安装了需要的shell，比如csh
```bash
$ which csh
/bin/csh
```

（2）为yourname用户设置shell类型
```bash
$ chsh -s /bin/csh yourname
```
