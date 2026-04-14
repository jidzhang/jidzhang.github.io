---
title: "配置 SciTE 编辑器用于 C/C++ 开发"
date: 2015-05-08
draft: false
slug: "configure-scite"
categories:
  - "工具"
---

## 前置准备

### 1. 安装 GCC

安装 MinGW 或 Dev-C++，确保 `gcc` 在 PATH 中：

```bash
gcc --version
```

### 2. 安装 AStyle（代码格式化）

从 [AStyle 官网](http://astyle.sourceforge.net/) 下载，放入 PATH。

## 配置 SciTE

打开 **选项 → 用户配置文件**（或编辑 `SciTEUser.properties`），添加：

```properties
# 自动补全
autocompleteword.automatic=1

# 字体设置
font.base=font:Consolas,size:12
font.monospace=font:Consolas,size:12

# 括号匹配
braces.sloppy=1

# C 程序：按 F5 自动编译并运行
command.go.needs.*.c=gcc -std=c99 "$(FileNameExt)" -o "$(FileName)"
command.go.*.c="$(FileName)"

# C++ 程序
command.go.needs.*.cpp=g++ -std=c++11 "$(FileNameExt)" -o "$(FileName)"
command.go.*.cpp="$(FileName)"

# 编译命令（Ctrl+F7）
command.compile.*.c=gcc -std=c99 -Wall "$(FileNameExt)" -o "$(FileName)"
command.compile.*.cpp=g++ -std=c++11 -Wall "$(FileNameExt)" -o "$(FileName)"
```

## 快捷键

| 快捷键 | 功能 |
|--------|------|
| `F5` | 编译并运行 |
| `Ctrl+F5` | 运行（不编译） |
| `Ctrl+F7` | 仅编译 |
| `Ctrl+I` | AStyle 格式化代码 |
| `Ctrl+D` | 复制当前行 |
| `Ctrl+Shift+↑/↓` | 移动当前行 |
