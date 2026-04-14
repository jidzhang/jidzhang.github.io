---
title: "Ubuntu 安装和卸载图形桌面"
date: 2015-05-09
draft: false
slug: "ubuntu-remove-and-install-gui-desktop"
categories:
  - "Linux"
---

## 安装桌面环境

| 桌面 | 安装命令 | 特点 |
|------|---------|------|
| GNOME | `sudo apt-get install ubuntu-desktop` | Ubuntu 默认，功能全面 |
| KDE | `sudo apt-get install kubuntu-desktop` | 类 Windows 风格，高度可定制 |
| XFCE | `sudo apt-get install xubuntu-desktop` | 轻量，适合旧硬件 |
| LXDE | `sudo apt-get install lubuntu-desktop` | 更轻量 |

安装后在登录界面可选择桌面环境。

## 卸载桌面环境

```bash
# 卸载 GNOME
sudo apt-get --purge remove liborbit2
sudo apt-get autoremove

# 卸载 KDE
sudo apt-get --purge remove kdelibs4c2a libarts1c2a
sudo apt-get autoremove

# 卸载 XFCE
sudo apt-get --purge remove xfce4 xfconf
sudo apt-get autoremove
```

> 卸载后重启即可进入纯命令行模式。
