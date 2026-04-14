---
title: "VMware Workstation 最小化到系统托盘"
date: 2015-01-11
draft: false
slug: "minimal-to-tray"
categories:
  - "Geek"
---

## 问题

VMware Workstation 最小化后仍然占据任务栏位置，无法像 VirtualBox 那样最小化到系统托盘。

## 解决方案

使用 **Trayconizer** 这个小工具（仅 4KB），可以让任何程序最小化到托盘。

### 步骤

1. 下载 [Trayconizer](http://www.whitsoftdev.com/trayconizer/)（选择 Unicode 版本）
2. 将 `Trayconizer.exe` 放到 `C:\Windows\` 目录下（方便任何位置调用）
3. 修改 VMware 快捷方式的**目标**为：

```
C:\Windows\Trayconizer.exe "C:\Program Files\VMware\VMware Workstation\vmware.exe"
```

> 注意：`Trayconizer.exe` 和后面的路径之间有一个空格。

4. 用修改后的快捷方式启动 VMware，最小化时就会自动收入托盘。

## 通用性

此方法不限于 VMware，**任何 Windows 程序**都可以用同样的方式最小化到托盘：

```
C:\Windows\Trayconizer.exe "目标程序的完整路径"
```
