---
title: "Vim 中文编码配置指南"
date: 2015-05-09
draft: false
slug: "vim-chinese-language-support"
categories:
  - "Vim"
---

## 编码相关选项

| 选项 | 缩写 | 作用 |
|------|------|------|
| `encoding` | `enc` | Vim 内部编码（Buffer、消息文字） |
| `fileencodings` | `fencs` | 打开文件时尝试的编码列表（按顺序猜测） |
| `fileencoding` | `fenc` | 当前文件的编码（保存时使用） |
| `termencoding` | `tenc` | 终端编码（终端模式下使用） |

## 编码转换流程

- **打开文件**：按 `fileencodings` 列表猜测编码 → 转换为 `encoding`
- **保存文件**：从 `encoding` 转换为 `fileencoding`
- **终端显示**：`termencoding` ↔ `encoding`

> 确保 Vim 编译时包含 `+multi_byte` 和 `+iconv`（`:version` 查看）。

## 常用配置

### 只编辑 UTF-8 文件

```vim
set encoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5
set fileencoding=utf-8
```

### Windows 环境（混合编码）

```vim
set encoding=cp936
set fileencodings=ucs-bom,utf-8,cp936,gb18030
set fileencoding=utf-8
```

### 终端环境额外设置

```vim
set termencoding=cp936   " 或 utf-8，与终端编码一致
```

## 常见问题

**Q: 为什么一次只能删除半个汉字？**

`encoding` 设置错误。确保 `encoding=utf-8` 或 `encoding=cp936`。

**Q: 如何将 GBK 文件另存为 UTF-8？**

```vim
:e file.txt          " 打开文件
:set fenc=utf-8      " 设置目标编码
:w                   " 保存
```

**Q: 为什么提示"不能转换"？**

Vim 编译时未包含 iconv 支持。需要重新编译。

**Q: `ucs-bom` 的作用？**

Windows 记事本保存 UTF-8 时会在文件头加 3 字节 BOM（EF BB BF）。`ucs-bom` 让 Vim 识别这种文件。Vim 保存 UTF-8 时会自动去掉 BOM。
