---
title: "Solaris 开启 FTP 和 SSH"
date: 2015-01-05
draft: false
slug: "solaris-enable-ftp-and-ssh"
categories:
  - ""
---

1. FTP

默认FTP是关闭的，启动命令：
```bash
# svcadm enable /network/ftp
```

此时查看FTP服务状态：
```bash
# svcs -l network/ftp
```

默认情况下，root用户无法登录，需要修改 `/etc/ftpd/ftpusers` 文件，把root那行前面加个#注释掉就可以了。

通常情况下不需要给root开启ftp，只给普通用户即可。因此通常不需要改配置文件，下面的SSH也是如此。

2. SSH

默认SSH是开启的。但是root用户无法登录，需要修改/etc/ssh/sshd_config，把里面的PermitRootLogin改为yes，再重启ssh服务，

重启ssh命令：
```bash
# svcadm restart network/ssh
```

如果重起命令不起作用 可以试下
```bash
svcadm refresh ssh
svcadm enable ssh
```
