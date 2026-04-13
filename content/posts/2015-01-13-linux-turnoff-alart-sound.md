---
title: "UNIX/Linux关掉报警声"
date: 2015-01-13
draft: false
slug: "linux-turnoff-alart-sound"
categories:
  - "Linux"
---

现在要关掉UNIX/Linux下的声音，尤其是在命令行下出现的嘟嘟报警声，可以用下面的命令：

方法一：
```
echo “set bell-style none”>> ~/.inputrc
```

然后logout、login即可生效

方法二：编辑 `/etc/inputrc`，将/etc/inputrc中的 `set bell-style none` 前的#去掉；

由于有的系统没有/etc/inputrc，因此推荐第一种办法。既安全有方便。

若还去除 Vi 中的铃声，需要 `echo “set vb t_vb=” >> ~/.vimrc`
