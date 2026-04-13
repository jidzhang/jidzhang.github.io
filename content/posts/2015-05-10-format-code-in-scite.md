---
title: "在Scite里用Astyle格式化代码"
date: 2015-05-10
draft: false
slug: "format-code-in-scite"
categories:
  - "Geek"
---

在 VC6.0 里很喜欢用 Alt+F8 的功能，它能够快速格式化代码，Scite中没有这个功能。今天看它的配置文件发现这么两行:
```
command.name.0.*.cxx=Indent
command.0.*.cxx=astyle -tapO $(FileNameExt)
```

用来缩进的? 查了一下 `astyle`, 原来我需要的就是这个功能.。

下载 [astyle](astyle.sourceforge.net) 最新版, 解压之后把bin目录下的 AStyle.exe 复制到C:/windows，

然后，修改一下配置文件 cpp.properties,（这一步可以忽略）， 如下:
```
command.name.0.*.cpp=Indent
command.0.*.cpp=astyle --style=ansi $(FileNameExt)
```

OK, 现在可以先选中文本然后`Ctrl+0`快速格式化代码了。


PS：

1.  其实要在Scite里面使用Astyle工具，只需要在astyle官网astyle中下载一个可执行程序astyle.exe，然后放到搜索路径中，比如C:/windows, 即可。修改配置文件是为了让格式化更漂亮，这里推荐用 `--style=ansi` 格式。
2.  AStyle安装使用可以阅读刚才下载文件里的相关文件（很详细）。
3.  Astyle用法： `astyle --style=ansi  filename.c`