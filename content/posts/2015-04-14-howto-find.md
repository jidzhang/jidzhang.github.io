---
title: "Linux find 命令完全指南"
date: 2015-04-14
draft: false
slug: "howto-find"
categories:
  - "Linux"
---

`find` 是 Linux 中最强大的文件搜索命令，支持按名称、类型、权限、时间、大小等多种条件查找。

## 基本语法

```bash
find [搜索路径] [查找条件] [执行动作]
```

## 常用查找条件

### 按名称

```bash
find /dir -name "*.c"           # 按文件名（区分大小写）
find /dir -iname "*.JPG"        # 不区分大小写
find . -name "*.log" -not -name "*.gz"   # 排除条件
```

### 按类型

```bash
find . -type f    # 普通文件
find . -type d    # 目录
find . -type l    # 符号链接
```

### 按时间

```bash
find / -mtime -5     # 5 天内修改过的文件
find / -mtime +3     # 3 天前修改过的文件
find / -mmin -30     # 30 分钟内修改过的文件
```

### 按大小

```bash
find . -size +100M       # 大于 100MB
find . -size 0c          # 空文件
find . -size +10k -size -1M  # 10KB ~ 1MB
```

### 按权限和用户

```bash
find . -perm 755          # 权限为 755
find . -user root         # 属主为 root
find /home -nouser        # 无有效属主
find . -group developers  # 属组
```

### 排除目录

```bash
find /apps -path "/apps/bin" -prune -o -print   # 排除 /apps/bin
```

## 执行动作

### -exec（对每个文件执行命令）

```bash
# 删除空文件
find . -size 0 -exec rm {} \;

# 列出文件详情
find . -type f -exec ls -l {} \;

# 删除 5 天前的日志
find /logs -type f -mtime +5 -exec rm {} \;
```

> `{}` 代表匹配到的文件，`\;` 是命令结束标记。

### -ok（交互式 -exec）

```bash
find . -name "*.conf" -mtime +5 -ok rm {} \;   # 删除前逐一确认
```

### xargs（推荐，更高效）

```bash
# 在所有 .c 文件中搜索关键字
find . -name "*.c" | xargs grep "TODO"

# 删除所有 core dump 文件
find / -name "core" | xargs rm -f

# 查看文件类型
find . -type f | xargs file
```

`xargs` 比 `-exec` 更高效：`-exec` 对每个文件启动一个进程，`xargs` 批量处理。

## 实用组合

```bash
# 查找最近 1 小时修改的 .py 文件
find . -name "*.py" -mmin -60

# 统计代码行数
find . -name "*.cpp" | xargs wc -l

# 查找大于 1G 的文件
find / -type f -size +1G 2>/dev/null

# 按修改时间排序
find . -type f -printf '%T@ %p\n' | sort -rn | head -20
```
