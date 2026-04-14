---
title: "cppman：在终端查看 C++ 文档的利器"
date: 2015-03-17
draft: false
slug: "recomment-cpp-manpages"
categories:
  - "Geek"
---

## 简介

[cppman](https://github.com/aitjcize/cppman) 从 cplusplus.com 抓取 C++ 标准库文档，以 man page 格式在终端中展示。比 libstdc++ 文档更方便，是 C++ 程序员的效率工具。

## 安装（Ubuntu）

```bash
# 1. 添加 PPA 源
sudo add-apt-repository ppa:aitjcize/manpages-cpp
sudo apt-get update

# 2. 安装
sudo apt-get install manpages-cpp

# 3. 缓存文档数据（首次需要，耗时较长）
cppman -c
```

## 使用

```bash
# 查询标准库组件
cppman cout
cppman vector
cppman string
cppman iterator

# 查看 STL 算法
cppman sort
cppman find_if

# 更新缓存
cppman -u
```

## 为什么推荐

- **直接按名称查询**：不用记 `std::` 前缀，`cppman cout` 即可
- **格式统一**：和 `man` 命令一样的阅读体验
- **离线可用**：缓存后无需联网
