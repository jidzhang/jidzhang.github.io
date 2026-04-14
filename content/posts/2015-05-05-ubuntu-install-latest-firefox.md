---
title: "Ubuntu 安装最新版 Firefox"
date: 2015-05-05
draft: false
slug: "ubuntu-install-latest-firefox"
categories:
  - "Linux"
---

## 方法一：PPA（旧版 Ubuntu）

```bash
sudo add-apt-repository ppa:mozillateam/firefox-stable
sudo apt-get update
sudo apt-get upgrade
```

## 方法二：直接安装（推荐，新版 Ubuntu）

```bash
sudo apt-get update
sudo apt-get install firefox
```

Ubuntu 22.04+ 的仓库会跟随最新稳定版，无需额外 PPA。

## 方法三：Snap 版

```bash
sudo snap install firefox
```

Ubuntu 22.04+ 默认使用 Snap 安装 Firefox，自动更新。
