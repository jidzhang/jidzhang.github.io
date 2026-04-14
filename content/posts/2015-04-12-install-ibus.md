---
title: "Ubuntu 安装 IBus 输入法框架"
date: 2015-04-12
draft: false
slug: "install-ibus"
categories:
  - "Linux"
---

## 最小化安装

```bash
sudo apt-get install ibus ibus-pinyin ibus-table ibus-gtk
```

安装后**必须注销并重新登录**才能使用。

## 常用输入法引擎

```bash
sudo apt-get install ibus-libpinyin      # 智能拼音（推荐）
sudo apt-get install ibus-pinyin         # 拼音
sudo apt-get install ibus-rime           # 中州韵（高级用户）
sudo apt-get install ibus-table-wubi     # 五笔
```

## 配置

```bash
ibus-setup    # 打开 IBus 偏好设置
im-config -s ibus   # 设为系统默认输入法框架
```

在弹出窗口中添加输入法，切换快捷键默认为 `Super+Space`。
