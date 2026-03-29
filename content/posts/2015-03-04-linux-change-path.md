---
title: "UNIX/Linux修改PATH变量"
date: 2015-03-04
draft: false
slug: "linux-change-path"
categories:
  - "Linux"
---

安装完操作系统（UNIX 或 Linux，尤其是Solaris这样的系统）后经常遇到：`command not found` 的错误提示, 原因：一是需要的程序可能没有安装（安装上就得了），二是程序不在搜索路径里。

如果程序不在搜索路径里，就需要手动修改PAThH，把可执行程序的路径加到PATH系统变量中。

首先，确认一下目前的PATH， 用命令

	echo $PATH

然后对缺失的path进行添加。方法如下：

对于Bash：

编辑home下的`.bashrc` 或`.profile` 或 `.bash_profile`，添加代码

	export PATH=$PATH:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:$HOME/bin

对于Csh:

编辑home下的.cshrc，添加代码

	set path=($path /bin /sbin /usr/bin /usr/sbin /usr/local/bin $home/bin)
