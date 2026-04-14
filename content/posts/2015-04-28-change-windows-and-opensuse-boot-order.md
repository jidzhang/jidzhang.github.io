---
title: "修改 openSUSE + Windows 双系统启动顺序"
date: 2015-04-28
draft: false
slug: "change-windows-and-opensuse-boot-order"
categories:
  - "Linux"
---

## 问题

先装 Windows 后装 openSUSE，GRUB 默认启动项变为 openSUSE。需要改回 Windows 优先。

## 方法一：YaST 图形界面

1. 打开 YaST → System → Boot Loader
2. 找到 Windows 启动项，设为 Default
3. 确定保存

## 方法二：编辑 GRUB 配置

```bash
sudo vi /boot/grub/menu.lst
```

找到 Windows 段落：

```bash
### Don't change this comment - YaST2 identifier: Original name: windows ###
title Windows
    rootnoverify (hd0,0)
    chainloader +1
```

将整个 Windows 段落**剪切到** openSUSE 段落的前面（即第一项），保存退出。

重启后 Windows 即为默认启动项。

> 新版 openSUSE 使用 GRUB2，配置文件为 `/etc/default/grub`，修改 `GRUB_DEFAULT` 后执行 `sudo grub2-mkconfig -o /boot/grub2/grub.cfg`。
