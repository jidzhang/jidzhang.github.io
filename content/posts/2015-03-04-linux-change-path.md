---
title: "Linux/Unix 修改 PATH 环境变量"
date: 2015-03-04
draft: false
slug: "linux-change-path"
categories:
  - "Linux"
---

## 问题

安装软件后执行命令提示 `command not found`，但程序确实已经安装。原因是程序所在目录不在 PATH 搜索路径中。

## 查看当前 PATH

```bash
echo $PATH
```

输出是以冒号分隔的目录列表，Shell 会依次在这些目录中查找可执行文件。

## 添加路径

### Bash（大多数 Linux 默认）

编辑 `~/.bashrc` 或 `~/.profile`，添加：

```bash
export PATH=$PATH:/新路径
```

例如：

```bash
export PATH=$PATH:/usr/local/bin:$HOME/bin
```

### C Shell

编辑 `~/.cshrc`，添加：

```csh
set path=($path /usr/local/bin $home/bin)
```

### 使配置生效

```bash
source ~/.bashrc    # Bash
source ~/.cshrc     # C Shell
```

或重新登录。

## 全局设置（所有用户）

- Bash：编辑 `/etc/profile` 或 `/etc/profile.d/` 下创建脚本
- 所有 Shell：编辑 `/etc/environment`（格式为 `PATH="/usr/local/sbin:/usr/local/bin:..."`）
