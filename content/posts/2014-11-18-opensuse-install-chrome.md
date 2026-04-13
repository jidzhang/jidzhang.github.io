---
title: "openSUSE安装Chrome浏览器"
date: 2014-11-18
draft: false
slug: "opensuse-install-chrome"
categories:
  - "Linux"
---

在openSUSE上安装chromium一点也不是问题，因为chromium是开源的，
源里直接有chromium。但是安装Chrome就不是那么自然了，毕竟chrome不开源。

这个时候需要先添加google的源，然后才可以安装chrome。如下：
```bash
sudo zypper ar -f http://dl.google.com/linux/chrome/rpm/stable/$(uname -m) Google-Chrome
sudo zypper ref
sudo zypper in google-chrome-stable
```
