---
title: "ubuntu remove and install Gui desktop"
date: 2015-05-09
draft: false
slug: "ubuntu-remove-and-install-gui-desktop"
categories:
  - "linux ubuntu"
---

# Ubuntu安装和卸载图形界面

通常的流程是这样：假如你已经默认安装了Gnome，现在想安装KDE或XFCE4，那么需要先卸载掉gnome，然后“在线”下载安装另一个桌面系统；如果你一开始用的是ubuntu-server版，直接安装就可以了，看下面。

## 卸载桌面

卸载gnome：
```
sudo apt-get --purge remove liborbit2
sudo apt-get autoremove
```

卸载kde：
```
sudo apt-get --purge remove kdelibs4c2a libarts1c2a
sudo apt-get autoremove
```

卸载xfce4：
```
sudo apt-get --purge remove kdelibs4c2a libarts1c2a
sudo apt-get autoremove
```

## 安装桌面

安装gnome：
```
sudo apt-get install ubuntu-desktop
```

安装kde：
```
sudo apt-get install kubuntu-desktop
```

安装xfce4：
```
sudo apt-get install xubuntu-desktop
```
