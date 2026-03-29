---
title: "控制 Linux 能否创建核心转储文件 coredump"
date: 2015-04-16
draft: false
slug: "linux-crate-coredump"
categories:
  - "Linux"
---

Linux系统能否创建核心转储文件coredump，以及core文件的大小限制，是由当前的shell控制，因此要通过相关shell进行修改：

（1）查看当前shell对核心文件的限制，

bash使用: `ulimit -c`，对于csh或tcsh, 使用`limit`进行查看

（2）修改对核心文件的限制，

对于bash，使用命令 `ulimit -c N` ，这里的N是核心文件的最大大小，以**千字节KB**为单位；N为0表示禁止创建核心文件；   
如果想取消对大小的限制，那么要用 ulimit -c unlimited，即可以无限大。

对于csh或tcsh，用命令 `limit coredumpsize N`，这里的N以**字节B**为单位。

（3）修改之后可以继续用（1）里的方法查看效果。
