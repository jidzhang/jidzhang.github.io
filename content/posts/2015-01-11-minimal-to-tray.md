---
title: "Vmware workstation最小化到托盘的解决方案"
date: 2015-01-11
draft: false
slug: "minimal-to-tray"
categories:
  - "Geek"
---

先下载 trayconizer.zip（我下载了个for unicode的版本，才4K）。

解压之后，放到硬盘的任何地方。

比如把Trayconizer.exe直接放到c:\windows目录下。

然后把vmware workstation的快捷方式的目标改为：

	c:\windows\Trayconizer.exe "C:\Program Files\VMware\VMware Workstation\vmware.exe"

注 意：trayconizer.exe后面应该有一个空格。

以后，用修改过的快捷方式打开的vmware，就可以最小化到系统托盘了。

当然了，该工具也适用于其他任何程序，最小化到托盘。

下载地址:

[http://www.whitsoftdev.com/trayconizer/](http://www.whitsoftdev.com/trayconizer/)
或
[http://www.uushare.com/user/antixeex/file/3115849](http://www.uushare.com/user/antixeex/file/3115849)
