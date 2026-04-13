---
title: "cygwin安装最新manpages"
date: 2015-03-02
draft: false
slug: "cygwin-install-lastest-manpages"
categories:
  - "Linux"
---

> 2026-04-13 修正错误

cygwin自身带的manpages不够全，比如 "man listen" 就没有，所以需要自己手动安装Linux的最新manpages。

下面有两个办法，推荐第一个：

<1> 下载并安装到系统中

下载最新的manpages资源 [http://www.kernel.org/pub/linux/docs/man-pages/](http://www.kernel.org/pub/linux/docs/man-pages/)，我下载的版本是3.41

解压到HOME中

    make gz ; make install  # 即可使用了


<2> 只下载不安装

下载最新的manpages资源http://www.kernel.org/pub/linux/docs/man-pages/，我下载的版本是3.41

解压到目录 %cygwin%\usr\man，%cygwin%就是cygwin的安装目录

在cygwin环境中编辑文件/etc/man.conf，增加一行
```
MANPATH    /usr/man/man-pages-3.41
```

保存退出。其中上面的数字3.41要根据版本号做修改

重启cygwin环境即可使用。


经过以上步骤，一般的man信息就都可以在本地查找到了(基本上是全部)。
