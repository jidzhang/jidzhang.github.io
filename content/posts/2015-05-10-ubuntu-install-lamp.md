---
title: "Ubuntu一键安装及卸载LAMP"
date: 2015-05-10
draft: false
slug: "ubuntu-install-lamp"
categories:
  - "Linux ubuntu"
---

一键安装LAMP服务：
```bash
sudo tasksel install lamp-server
```

一键卸载LAMP：
```bash
sudo tasksel remove lamp-server
```

补充：
```bash
LAMP = Linux+Apache+MySQL+PHP
```

通过上面的命令卸载Lamp时不免把Linux系统本身的东西卸载掉了，因此，

在卸载LAMP后**一定**记着更新一下系统：
```bash
sudo apt-get update
sudo apt-get upgrade
```

上面两条都要执行，切记！

参考资料：[Ubuntu Skills](http://wiki.ubuntu.org.cn/UbuntuSkills#.E4.B8.80.E9.94.AE.E5.AE.89.E8.A3.85_LAMP_.E6.9C.8D.E5.8A.A1)
