---
title: "停止和删除Hasplms服务"
date: 2014-04-16
draft: false
slug: "delete-hasplms-service"
categories:
  - ""
---

使用圣天诺HASP加密的软件都会启动一个叫做Hasplms或Sentinel LDK License Manager的服务项，这个服务是软件运行所必须的。但是在软件卸载后往往这个服务还在运行，重启电脑后也还继续运行。这就非常不厚道了。

针对这个情况可以直接在命令行上停止这个服务并彻底删除或者通过hasp的运行时管理工具彻底删除hasp服务：

###在命令行删除hasplms服务

1. 通过开始菜单，打开命令行cmd
2. 停止服务，并设置不自动启动。在命令行内依次输入下面两个命令
		sc stop "hasplms"
		sc config "hasplms" start=disabled
3. 删除服务
		sc delete "hasplms"

P.S. 这个方法用于普通用户直接快速的解决问题，而对于软件的发行者，更应该考虑怎么在软件卸载的时候把Hasplms服务彻底删除。

###通过hasp运行时管理工具删除hasplms服务

1. 首先到Sentinel官网下载HASP运行环境管理工具
http://sentinelcustomer.safenet-inc.com/sentineldownloads/?s=&c=End+User&p=Sentinel+HASP&o=all&t=all
这里推荐Command Line Run-time installer，因为这个工具的命令行非常强大。其实这个管理工具就是一个exe: haspdinst.exe，同时包括一个帮助文档：readme.html。

2. 下载后解压到硬盘的随便一个位置，比如C盘。

3. 通过开始菜单打开命令行（Windows 7用户注意选择管理员账户运行cmd），然后cd到Sentinel工具目录，务必找到haspdinst.exe。

**查看haspinst帮助**

	haspdinst -h

**卸载HASP运行环境**
	
	haspdinst -fr -purge

**重新安装HASP运行环境**
	
	haspdinst -i -fi -kp


【P.S】原文同步在[cnblogs](http://www.cnblogs.com/ingvar/p/3663246.html)上。
