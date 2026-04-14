---
title: "Firefox 修改 Backspace 键行为"
date: 2015-04-11
draft: false
slug: "modify-firefox-backspace-key"
categories:
  - "Geek"
---

## 问题

Firefox 在 Linux/macOS 上，Backspace 键默认不执行"后退"操作。Windows 用户习惯用 Backspace 后退。

## 解决

地址栏输入 `about:config`，搜索 `browser.backspace_action`，双击修改值：

| 值 | 行为 |
|------|------|
| `0` | Backspace = 后退，Shift+Backspace = 前进 |
| `1` | Backspace = 页面向上滚动 |
| `2` | Backspace 无操作（默认） |

修改后立即生效。

## 其他导航快捷键

| 快捷键 | 功能 |
|--------|------|
| `Alt + ←` | 后退 |
| `Alt + →` | 前进 |
| `Ctrl + [` | 后退（部分系统） |
| `Ctrl + ]` | 前进（部分系统） |
