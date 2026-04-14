---
title: "Linux 下编译安装 tar.bz2 软件包"
date: 2015-01-12
draft: false
slug: "linux-install-tarbz2-file"
categories:
  - "Linux"
---

## 标准流程

以源码包 `foo-1.0.tar.bz2` 为例：

```bash
# 1. 解压
tar jxvf foo-1.0.tar.bz2

# 2. 进入源码目录
cd foo-1.0

# 3. 配置（指定安装路径，方便卸载）
./configure --prefix=/opt/foo

# 4. 编译
make

# 5. 安装（通常需要 root）
sudo make install
```

## 各步骤说明

| 步骤 | 作用 | 说明 |
|------|------|------|
| `tar jxvf` | 解压 | `j` 表示 bzip2，如果是 `.tar.gz` 则用 `tar zxvf` |
| `./configure` | 检查环境、生成 Makefile | `--prefix` 指定安装路径，不指定则默认装到 `/usr/local` |
| `make` | 编译源码 | 根据 Makefile 编译生成可执行文件 |
| `make install` | 安装到系统 | 将编译产物复制到 `--prefix` 指定的目录 |

## 卸载

```bash
# 如果源码目录还在
sudo make uninstall

# 或者直接删除安装目录
sudo rm -rf /opt/foo
```

## 常见问题

- **configure 报错缺少依赖**：根据错误提示安装对应的 `-dev` 或 `-devel` 包
- **make 报错**：检查 GCC 是否安装（`gcc --version`）
- **权限不够**：`make install` 需要 root 权限，使用 `sudo`
