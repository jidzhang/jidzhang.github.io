---
title: "FreeBSD开启SSH登录"
date: 2015-04-18
draft: false
slug: "freebsd-enable-ssh-login"
categories:
  - "Linux"
---

（1）编辑/etc/inetd.conf, 去掉ssh前的#

（2）编辑/etc/rc.conf
  确保有这一行 `sshd_enable="yes"`

（3）最后配置sshd(守护进程)，编辑文件 `/etc/ssh/sshd_config`， 在文件后加入下面的三行

	PermitRootLogin yes   #允许root

	PermitEmptyPasswords no    #不允许空密码
	PasswordAuthentication yes    #认证


（4）激活sshd服务：

	root#   /etc/rc.d/sshd  restart
