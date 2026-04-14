---
title: "Linux：创建和删除以连字符开头的文件"
date: 2015-04-29
draft: false
slug: "linux-create-a-file-named--i"
categories:
  - "Linux"
---

## 问题

文件名以 `-` 开头时，命令会将其误认为选项：

```bash
touch -i      # 报错：invalid option
rm -i         # 报错：invalid option
```

转义 `\` 或引号都不能解决。

## 解决方法

### 方法一：加路径前缀

```bash
touch ./-i
rm ./-i
```

### 方法二：用 `--` 分隔符

```bash
touch -- -i
rm -- -i
```

`--` 是 POSIX 标准约定的选项终止符，告诉命令"后面都不是选项了"。

方法二更通用，推荐。
