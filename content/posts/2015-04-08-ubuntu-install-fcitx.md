---
title: "Ubuntu 安装 Fcitx / 搜狗输入法"
date: 2015-04-08
draft: false
slug: "ubuntu-install-fcitx"
categories:
  - "Linux"
---

## 步骤

### 1. 卸载 IBus（可选，避免冲突）

```bash
sudo apt-get purge ibus ibus-*
```

### 2. 添加 Fcitx PPA 源

```bash
sudo add-apt-repository ppa:fcitx-team/nightly
sudo apt-get update
```

### 3. 安装输入法

**搜狗拼音（推荐）：**

```bash
sudo apt-get install fcitx-sogoupinyin
```

**其他可选引擎：**

```bash
sudo apt-get install fcitx-googlepinyin   # Google 拼音
sudo apt-get install fcitx-sunpinyin      # Sun 拼音
sudo apt-get install fcitx-table fcitx-wubi  # 五笔
```

### 4. 重启

注销并重新登录，或重启系统。首次登录时系统会提示选择输入法框架，选择 Fcitx。

## 配置

点击托盘区键盘图标 → 配置 → 添加输入法，选择需要的输入法即可。

## 常见问题

- **托盘不显示输入法图标**：安装 `fcitx-config-gtk3`
- **无法切换输入法**：检查 `im-config` 是否设置为 fcitx
