---
title: "把SciTe打造成更强大的程序员专用编辑器"
date: 2015-05-08
draft: false
slug: "configure-scite"
categories:
  - "Geek"
---
SciTe是一款很小巧又很强大的编辑器，非常适合程序员使用。

大家了解Scite通常是因为使用AutoIt才间接知道Scite，其实，Scite的使用范围要广得多。

现在开始配置Scite，使得它更适合编写调试程序，

（1）首先安装GCC编译器，并且把gcc程序放在path里

怎么安装gcc呢？可以安装MinGW，可以安装Dev-Cpp之类的包含gcc的IDE，关键是把gcc程序目录加入path

（2）安装AStyle，代码格式化工具

参考本空间内文章：http://hi.baidu.com/linccn/blog/item/65916110da1f0566ca80c4ee.html

或许你已经安装了Astyle，但我用的是原版Scite，所以需要单独安装Astyle。

（3）编辑“选项”->“用户配置文件”或“本地配置文件”，加入以下代码：


	#################
	#以下取自全局设置.properties
	#################
	#括号匹配模式
	#braces.sloppy=0

	#默认文件名后缀
	default.file.ext=.txt

	#代码自动补全
	autocompleteword.automatic=1

	#编辑窗口字体
	font.base=font:Courier New,size:12

	#使用等宽字体
	font.monospace=font:Verdana,size:10

	#################
	#以下取自cpp.properties
	#################

	#To make the Go command both compile(if needed) and execute, use this setting:
	#对C程序，运行脚本命令（即F5），可以在需要的时候先编译后执行，默认行为是只运行不编译。如果之前没有用Ctrl+F7编译过，则无法运行。
	command.go.needs.*.c=gcc $(ccopts) -std=c99 $(FileNameExt) -o $(FileName)
