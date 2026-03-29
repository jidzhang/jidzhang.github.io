---
title: "ubuntu安装dos2unix工具"
date: 2015-02-10
draft: false
slug: "ubuntu-instal-dos2unix"
categories:
  - "Linux"
---

ubuntu下默认没有安装dos2unix工具，而且也没有一个叫这个名字的工具（我在solaris里用过dos2unix，不知道为啥ubuntu没有）。   
但是有一个替代工具——tofrodos , 下面就说一下它的安装和使用。

（1）安装tofrodos

	sudo apt-get install tofrodos

实际上它安装了两个工具：todos（相当于unix2dos）和fromdos（相当于dos2unix）

安装完即可，现在你已经可以进行文本格式的转换啦。

	比如： todos Hello.txt (即unix2dos Hello.txt)
	fromdos Hello.txt (即dos2unix Hello.txt)

（2）做一些优化

由于习惯了unix2dos和dos2unix的命令，可以把上面安装的两个工具链接成unix2dos和dos2unix，或者仅仅是起个别名，并放在启动脚本里。

步骤：

	ln -s /usr/bin/todos /usr/bin/unix2dos
	ln -s /usr/bin/fromdos /usr/bin/dos2unix

或者在 ~/.bashrc里起个别名
	
	vi ~/.bashrc

添加 
	
	alias unix2dos=todos 
	alias dos2unix=fromdos
