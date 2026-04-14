---
title: "openSUSE 安装 Google Chrome 浏览器"
date: 2014-11-18
draft: false
slug: "opensuse-install-chrome"
categories:
  - "Linux"
---

Chromium（开源版）可以直接从 openSUSE 源安装，但 Chrome（Google 官方版）需要手动添加 Google 的软件源。

## 安装步骤

```bash
# 1. 添加 Google Chrome 源
sudo zypper ar -f http://dl.google.com/linux/chrome/rpm/stable/$(uname -m) Google-Chrome

# 2. 刷新源
sudo zypper ref

# 3. 安装
sudo zypper in google-chrome-stable
```

## 安装 Chromium（开源版）

```bash
sudo zypper in chromium
```

## 区别

| | Chromium | Chrome |
|---|---------|--------|
| 开源 | ✅ | ❌ |
| 自动更新 | 通过系统源 | 通过 Google 源 |
| PDF 查看器 | 无内置 | 内置 |
| 编解码器 | 部分缺失 | 完整 |
