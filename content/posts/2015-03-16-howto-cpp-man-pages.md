---
title: "C++ man pages 的使用方法"
date: 2015-03-16
draft: false
slug: "howto-cpp-man-pages"
categories:
  - "Linux"
---

> 前提：已安装 C++ man pages，参见[上篇](../15/linux-install-cpp-man-pages.html)。

## 问题

`man cout` 提示 `No manual entry for cout`，无法直接查询 C++ 标准库函数。

## 解决方法

C++ man pages 按命名空间和头文件组织，查询格式为：

```bash
man 命名空间::头文件名
```

### 示例

```bash
# 查询 cout —— 属于 std 命名空间，定义在 iostream 中
man std::iostream
# 打开后按 /cout 搜索

# 查询 slist —— 属于 __gnu_cxx 命名空间
man __gnu_cxx::slist

# 查询 vector
man std::vector
```

## 与 C 语言 man pages 的对比

| 操作 | C 语言 | C++ |
|------|--------|-----|
| 查询 printf | `man 3 printf` | — |
| 查询 cout | — | `man std::iostream` 后搜索 |
| 章节指定 | `man 2 open`（系统调用） | `man std::vector` |
| 直接按函数名查 | ✅ | ❌ 需要知道所属头文件 |

> 注意：C++ man pages 由 Doxygen 生成，描述可能不如 C 的 man pages 详细。
