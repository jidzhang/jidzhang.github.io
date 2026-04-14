---
title: "RPM 包管理常用命令"
date: 2015-04-10
draft: false
slug: "linux-install-rpm-packages"
categories:
  - "Linux"
---

## 基本操作

### 安装

```bash
rpm -ivh package-1.0.i386.rpm
```

| 选项 | 含义 |
|------|------|
| `-i` | 安装 |
| `-v` | 显示详细信息 |
| `-h` | 显示进度条 |

### 卸载

```bash
rpm -e package_name    # 注意：用包名，不是文件名
```

### 升级

```bash
rpm -Uvh package-2.0.i386.rpm
```

升级会自动卸载旧版本。如果配置文件不兼容，旧文件会保存为 `.rpmsave`。

降级安装需加 `--oldpackage`。

---

## 查询

```bash
# 查询已安装的包
rpm -q package_name

# 列出所有已安装的包
rpm -qa

# 查询某个文件属于哪个包
rpm -qf /path/to/file

# 查看包信息（描述、大小、开发者等）
rpm -qi package_name

# 列出包包含的文件
rpm -ql package_name

# 查看未安装的 RPM 文件的信息
rpm -qpi package-1.0.i386.rpm
rpm -qpl package-1.0.i386.rpm
```

---

## 校验

```bash
# 校验所有已安装包的文件完整性
rpm -Va

# 校验指定包
rpm -V package_name
```

校验结果中的标记含义：

| 标记 | 含义 |
|------|------|
| `5` | MD5 校验失败 |
| `S` | 文件大小改变 |
| `T` | 修改时间改变 |
| `M` | 权限/类型改变 |
| `U`/`G` | 用户/组改变 |

---

## 实用技巧

```bash
# 远程安装（通过 FTP/HTTP）
rpm -ivh http://example.com/package-1.0.i386.rpm

# 强制安装（忽略依赖）
rpm -ivh --nodeps package-1.0.i386.rpm
```
