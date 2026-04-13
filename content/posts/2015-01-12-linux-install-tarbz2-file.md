---
title: "Linux下安装tar.bz2文件方法"
date: 2015-01-12
draft: false
slug: "linux-install-tarbz2-file"
categories:
  - "Linux"
---

Linux下解压安装tar.bz2文件

下面所所有操作，后面有所有步骤说明
```bash
[yonghu@localhost ~]# su root
```

命令:                                        
```bash
[root@localhost ~]# cd /home/new/Desktop
[root@localhost Desktop]# tar jxvf fcitx-3.4.2.tar.bz2
[root@localhost Desktop]# cd /home/new/Desktop/fcitx-3.4.2
[root@localhost fcitx-3.4.2]# ./configure --prefix=/opt/fictx
[root@localhost fcitx-3.4.2]# make
[root@localhost fcitx-3.4.2]# make install
```

各步骤详解

[yonghu@localhost ~]# su root       //(使用root帐户登录，使用其他用户，之后操作有可能权限不够)

命令:                                             // 输入root密码

[root@localhost ~]# cd /home/new/Desktop         // (切换到tar.bz2文件所在目录，这里我的tar.bz2文件在桌面)

[root@localhost Desktop]# tar jxvf fcitx-3.4.2.tar.bz2  // (解压tar.bz2文件，这里以fcitx-3.4.2来举例，解压得到fcitx-3.4.2文件夹)

[root@localhost Desktop]# cd /home/new/Desktop/fcitx-3.4.2    //（切换目录到fcitx-3.4.2，软件解压的目录）

[root@localhost fcitx-3.4.2]# ./configure --prefix=/opt/fictx  //(配置，把文件存放在/opt/fictx下，删除时，卸载软件时，只要删除这个文件就行了)

[root@localhost fcitx-3.4.2]# make (编译)

[root@localhost fcitx-3.4.2]# make install (安装)
