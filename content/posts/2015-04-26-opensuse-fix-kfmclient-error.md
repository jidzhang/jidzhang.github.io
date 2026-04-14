---
title: "openSUSE: could not find 'kfmclient executable' 解决方法"
date: 2015-04-26
draft: false
slug: "opensuse-fix-kfmclient-error"
categories:
  - "Linux"
---

## 问题

openSUSE 中某些应用报错 `could not find 'kfmclient executable'`。

## 原因

`kfmclient` 是 KDE 文件管理器 Konqueror 的命令行工具，用于打开 URL 和文件。系统缺少 Konqueror 导致找不到此命令。

## 解决

```bash
sudo zypper install konqueror
```

安装后 `kfmclient` 命令即可使用。
