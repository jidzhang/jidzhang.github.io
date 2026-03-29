---
title: "man : 在线手册页，查询系统命令、库函数帮助"
date: 2015-03-06
draft: false
slug: "howto-man-command"
categories:
  - "Linux"
---

man -- format and display the on-line manual pages

常用法：

	man [section] name

其中:

* section 指的是手册页的哪个部分，可以是1、2、3…8.，若不指定，man会按照次序依次查找，知道找到第一个。

* name 指的是某个命令、函数或文件

下面对section做一些说明：

   1 = 命令（比如cp mv rm 等）
   2 = 系统调用 （比如openread close 等）
   3 = C库函数 （比如printf ）
   4 = 设备和特殊文件
   5 至 8省略， 详细说明请查看： man man 

例子：

	man cp   => man 1 cp,  1通常省略
	man open => man 2 open, 但如果用 `man 3 open` 的话就出错：No entry for open in section 3 of the manual
	man printf  

注意：这个不同于 man 3 printf, 因为在用户命令里面也有一个控制格式出错的printf命令，所以优先显示的user command : printf, `man 3 printf`这个才是真正的查询C库函数里的printf函数

另外，manpages 有个主页：[http://www.kernel.org/pub/linux/docs/man-pages/](http://www.kernel.org/pub/linux/docs/man-pages/)  提供最新的manpages。
