---
title: "solaris set NFS"
date: 2015-02-19
draft: false
slug: "solaris-set-nfs"
categories:
  - ""
---

以下操作均需root权限

## server端：

（1）开启守护进程：

	svcadm enable network/nfs/server (solaris 10)
	/etc/init.d/nfs.server start  (solaris 10及以前版本)

（2）共享目录，按照如下格式：

	share [-F fstype] [ -o options] [-d "<text>"] <pathname> [resource] 或
	share -F nfs -o rw=engineering -d "home dirs" /export/home2 或
	share -F nfs -d "my shared dir" /export/home/shared_dir

注意，以上的命令可以即时生效，但是系统重启后就没了。
所以，最好再写进启动脚本里，只要把上面的share代码完完整整添入/etc/dfs/dfstab即可。

（3）检查share是否成功

	dfshares 或
	showmount -e

## client端：

（1）开启守护进程：
	
	svcadm enable network/nfs/server (solaris 10)
	/etc/init.d/nfs.server start  (solaris 10及以前版本)

（2）进行挂载，与前面的相似，也是有两个办法

命令行：`mount -F nfs ServerIP:/SharedPath ClientPath`

可以即时生效，但是重启后就失效了。

加入启动脚本，编辑文件 /etc/vfstab, 添加

	ServerIP:/SharedPath - ClientPath nfs - yes -

注意上面的短横线两边都有空格。

（3）查看挂载状态
	
	showmount       (没有参数，列出所有挂载了共享目录的客户端client)
	showmount -a  (列出server上共享的目录，同时列出client上的挂载点)
	showmount -d  (列出被client挂载的目录)
	showmount -e  （列出server端的共享目录）

PS:更过showmount的使用请见本空间
