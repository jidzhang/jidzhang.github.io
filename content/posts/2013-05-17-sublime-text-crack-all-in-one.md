---
title: "极速破解sublime text全部版本"
date: 2013-05-17
draft: false
slug: "sublime-text-crack-all-in-one"
categories:
  - "Hack"
---

## 摘要
本篇文章介绍一种非常直接、快速地破解[Sublime Text](http://www.sublimetext.com)编辑器的方法,并且这里的方式适用于Sublime Text2/3(以下为了方便简称为ST2,ST3)的所有版本和几乎所有平台(Win32,Win64,Linux32,Linux64,除了MacOS)。下面是破解后的效果图:

ST2217-Windows

![ST2-Cracked](https://dl.dropboxusercontent.com/u/6893139/images/2013-05-20-st2217-cracked.png)

ST3008-Windows

![ST3-Cracked](https://dl.dropboxusercontent.com/u/6893139/images/2013-05-20-st3008-cracked.png)

ST3033-Linux

![ST3033-Linux-Cracked](https://dl.dropboxusercontent.com/u/6893139/images/2013-05-21-st-3033-cracked.png)

## 准备工具
1. 十六进制编辑器,推荐WinHex,或其他可以进行十六进制搜索的任意编辑器,比如Vim,Sublime Text, UltraEditor等。
2. 可以用正则表达式进行文本搜索的编辑器,比如Vim,Sublime Text等。
3. 汇编器,windows下可以用OllyDBG或IDA,Linux下用objdump就可以。

## 破解过程
下面的方法是通用的,适用于Windows或Linux上的所有Sublime Text,在方法上仅仅是开始的时候有点区别。下面分别针对Windows系统和Linux系统做详细说明:

### <0> 下载并安装sublime_text
自己到官网上下载[Sublime Text 2](http://www.sublimetext.com/2) 或 [Sublime Text 3](http://www.sublimetext.com/3), 然后安装。
安装后把可执行程序备份一遍。比如把sublime_text.exe复制一份为sublime_text.orig.exe。
### <1.1> sublime_text for Windows
用OllyDBG把sublime_text.exe加载起来,然后把OllyDBG里面的文字全部复制出来,保持为st-crack.asm。(这是为了进行搜索)
### <1.2> sublime_text for Linux
1. 先要知道你的ST的真正的可执行程序在哪里:
```
$ which sublime_text | xargs ls -l
>> .....
```
上面的代码告诉你真正的ST在哪里。下面复制一份到Home下面
```
cp ...
```

2. 用objdump分析上面找到的ST
```
objdump -d ST_file > st-crack.asm
```

### <2> 汇总信息
上一步得到了反汇编文件 st-crack.asm，接下来统一分析。

### <3> 分析st-crack.asm
用编辑器打开st-crack.asm,然后搜索这个正则表达式
```
(.*<je>.*\n.*<cmp>.*\n){8,}
```

上面这个正则其实是搜索一大片的
```
je....
cmp...
je...
cmp...
je...
cmp...
je....
cmp...
je...
cmp...
je...
cmp...
```

因为找到的这段函数是验证序列号的最重要的函数(这个规律是我在调试的时候发现的,对ST2,ST3都适用。),这个函数用于判断注册码是否正确,返回值是一个布尔值,**只要改了这个函数的返回值,ST就被完美爆破了**。
### <4> 着手修改上面的函数的返回值
上面已经找到了关键函数段,往下找,一直找到函数返回: **C3    retn**,
然后往上数两行,看到**mov al, bl**,就是它了,记下它的二进制串(**一定以自己搜出来的字符串为准**):
```
8B4C2478 8AC3 5B
```
把它修改了就可以了。

在这里我们的目的是修改为mov al, 0x1(sublime_text-2)或xor eax, eax(sublime_text_3)。
```
mov al, 0x1 的二进制是B001
xor eax, eax的二进制是33C0
```

### <5> 修改可执行程序（用WinHex或其他十六进制编辑器）
找到第4步的**mov eax, ecx**非常关键,如果没有找到请重新阅读步骤1-4.

用WinHex打开sublime_text.exe,然后搜索十六进制串:
```
8B4C2478 8AC3 5B（以你自己找到的字串为准，因为不同的版本会不一样）
```
把8BC3改为B001(ST2)或33C0(ST3),保存文件。

运行sublime text,看看是不是被完美破解了!!


