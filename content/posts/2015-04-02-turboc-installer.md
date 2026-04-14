---
title: "怀旧：Turbo C 经典版本回顾"
date: 2015-04-02
draft: false
slug: "turboc-installer"
categories:
  - "Geek"
---

## Turbo C 的历史

Turbo C 是 Borland 公司推出的 C 语言集成开发环境，在 DOS 时代是学习 C 语言的标准工具。

| 版本 | 年份 | 主要特性 |
|------|------|---------|
| Turbo C 1.0 | 1987 | 全新集成开发环境，编辑/编译/运行一体化 |
| Turbo C 1.5 | 1988 | 增加图形库和文本窗口函数库 |
| Turbo C 2.0 | 1989 | 增加查错功能，Tiny 模式生成 .COM 文件 |
| Turbo C 2.01 | 1989 | Bug 修复版 |
| Turbo C++ 3.0 | 1992 | 面向对象，C++ 支持 |
| Borland C++ | 1991 | Windows 3.0 支持，新一代产品 |

Turbo C 2.0 因性能稳定，在教育领域使用了近二十年，是很多人的 C 语言启蒙工具。

## 使用方式

- **安装版**：运行 `INSTALL.EXE`，会自动设置目录结构
- **免安装版**：解压后直接运行 `TC.EXE`，建议放在 `D:\TURBOC2`（路径不能有中文和空格）

## 在现代 Windows 上运行

Turbo C 是 16 位 DOS 程序，64 位 Windows 无法直接运行。推荐使用：

- **DOSBox** — DOS 模拟器，兼容性最好
- **DOSBox-X** — DOSBox 增强版
