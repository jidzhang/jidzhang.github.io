---
title: "关闭 Linux 命令行和 Vim 的报警声（Beep）"
date: 2015-01-13
draft: false
slug: "linux-turnoff-alart-sound"
categories:
  - "Linux"
---

## 命令行 Beep 声

在终端中按 Tab 补全或遇到错误时发出"嘟"声，两种关闭方式：

**方法一（推荐，仅影响当前用户）：**

```bash
echo "set bell-style none" >> ~/.inputrc
```

重新登录后生效。

**方法二（全局生效）：**

编辑 `/etc/inputrc`，去掉 `set bell-style none` 前面的 `#`。如果文件不存在则用方法一。

## Vim Beep 声

```bash
echo "set vb t_vb=" >> ~/.vimrc
```

这会将 Vim 的蜂鸣替换为视觉闪烁（visual bell），并将闪烁设为空（即完全静默）。
