---
title: "Vim 与系统剪贴板的复制粘贴"
date: 2015-05-01
draft: false
slug: "copy-between-vim-and-os"
categories:
  - "Vim"
---

## 快速上手

| 操作 | 快捷键 |
|------|--------|
| Vim → 系统剪贴板 | 选中内容后 `"+y` |
| 系统剪贴板 → Vim | 正常模式下 `"+p` 或 `Shift+Insert` |

## 原理

Vim 有多个寄存器（用 `:reg` 查看），其中 `+` 寄存器与系统剪贴板关联。

| 寄存器 | 用途 |
|--------|------|
| `"` | 未命名寄存器（默认，`y`/`p` 直接使用） |
| `0` | 最近一次 `y` 复制的内容 |
| `1-9` | 最近删除/修改的内容 |
| `a-z` | 命名寄存器（用户自定义） |
| `+` | **系统剪贴板** |

## 操作方式

```vim
" 复制到指定寄存器："ay（复制到寄存器 a）
" 粘贴指定寄存器："ap（粘贴寄存器 a 的内容）

" 系统剪贴板操作：
"+y     " 复制选中内容到系统剪贴板
"+p     " 粘贴系统剪贴板内容
```

> 前缀 `"` 是寄存器标识符，不是字符串引号。

## 注意事项

Ubuntu、openSUSE 等默认安装的 `vim-tiny` 不支持系统剪贴板。需要安装完整版：

```bash
# Ubuntu
sudo apt-get install vim-gtk    # 或 vim-gnome / vim-athena

# 验证是否支持
vim --version | grep clipboard
# 应显示 +clipboard
```
