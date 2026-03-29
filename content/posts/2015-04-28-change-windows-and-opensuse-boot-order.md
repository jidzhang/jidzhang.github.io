---
title: "修改openSUSE与Windows多系统启动顺序"
date: 2015-04-28
draft: false
slug: "change-windows-and-opensuse-boot-order"
categories:
  - "Linux openSUSE"
---

由于先安装了Windows后安装openSUSE，结果启动项首选项由windows变成了openSUSE，这一点很出乎预料。

那么怎么恢复回来呢？

其实，在openSUSE系统里有一个Boot Loader的设置项，可以更改启动顺序。可以通过图形界面设置，也可以直接编辑配置文件。

（1）图形界面方式设置

系统菜单-->Applications-->System-->Administor Settings-->System-->Boot Loader 单击后打开设置界面

把windows调到第一项，并设置为默认项（Set as Default）。确定即可。

（2）编辑 /boot/grub/menu.list

	sudo vi /boot/grub/menu.list

找到Windows行，类似下面的四行（）

	###Don't change this comment - YaST2 identifier: Original name: windows###
	title Windows
	    rootnoverify (********)
	    chainloader +1

剪切到 title Desktop -- openSUSE 11.****这一行的注释行的前面，然后保存、退出，即可。

剩下的就是重启机器啦。

转自: [CSDN](http://blog.csdn.net/ingvar08/article/details/6817255)

