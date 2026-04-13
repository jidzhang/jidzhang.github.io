---
title: "修改sublime_text主题(Solarized)"
date: 2013-07-13
draft: false
slug: "modify-sublime_text-theme"
categories:
  - "Hack"
---

最近发现*Solarized*主题很耐看，尤其是Dark主题喜欢得不行。所以把常用的工具的主题都设置成了*Solarized(Dark)*样式。

但是，sublime_text自带的Solarized(Dark)有一点让人不满意：
*选中文本的颜色太浅，几乎分辨不出来。*

故而决定修改sublime_text的Solarized(Dark)主题。

> 2026-04-13 修正错误

修改方法：

1. 在sublime_text程序所在目录下的Packages目录下找到Color Scheme-Default.sublime-package。（注：.sublime-package文件其实是一个文件包，类似于.zip文件）
打开**Color Scheme-Default.sublime-package**（打开方式就是打开zip文件的方法），在里面找到文件**Solarized (Dark).tmTheme**，打开该文件。

2. 修改Solarized (Dark).tmTheme

把第23行的**<string>#0A2933</string>** 改为 **<string>#EEE8D5</string>**，保存退出，重新打开sublime_text，看一下效果。

*OVER！*

3. 补充：关于sublime_text的Packages

目前的sublime_text的工具及插件流行使用.sublime-package后缀的压缩文件，而不是像原来sublime_text2的时候把程序存放在文件夹中。这是sublime_text3很大的不同点。
以上的修改方法完全适用于sublime_text2/sublime_text3。

