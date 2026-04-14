---
title: "在 SciTE 中用 AStyle 格式化代码"
date: 2015-05-10
draft: false
slug: "format-code-in-scite"
categories:
  - "工具"
---

## 安装 AStyle

1. 从 [astyle.sourceforge.net](http://astyle.sourceforge.net) 下载
2. 将 `AStyle.exe` 复制到 `C:\Windows\`（或任意 PATH 目录）

## 配置 SciTE

编辑 `cpp.properties`（或用户配置文件），添加：

```properties
# Ctrl+0 格式化当前文件
command.name.0.*.cpp=Format Code
command.0.*.cpp=astyle --style=ansi $(FileNameExt)

# 同样适用于 .c 文件
command.name.0.*.c=Format Code
command.0.*.c=astyle --style=ansi $(FileNameExt)
```

## 使用

- **Ctrl+0**：格式化整个文件
- 先选中文本再按 Ctrl+0：格式化选中部分

## AStyle 常用风格

```bash
astyle --style=ansi     file.c    # ANSI 风格（推荐）
astyle --style=kr       file.c    # K&R 风格
astyle --style=linux    file.c    # Linux 内核风格
astyle --style=google   file.c    # Google 风格
```

命令行直接用：

```bash
astyle --style=ansi *.c *.cpp    # 批量格式化
```
