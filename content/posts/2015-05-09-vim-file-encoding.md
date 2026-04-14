---
title: "Linux 下文件编码查看与转换"
date: 2015-05-09
draft: false
slug: "vim-file-encoding"
categories:
  - "Linux"
---

## 查看文件编码

### Vim 中查看

```vim
:set fileencoding
" 显示当前文件编码，如 utf-8 或 cp936
```

### 命令行查看

```bash
file -i filename
enca -L zh_CN filename    # 需安装 enca
```

## 文件编码转换

### 方法一：Vim 内转换

```vim
:e file.txt
:set fenc=utf-8    " 设置目标编码
:w                 " 保存
```

### 方法二：iconv 命令

```bash
# GBK → UTF-8
iconv -f GBK -t UTF-8 file_gbk.txt -o file_utf8.txt

# UTF-8 → GBK
iconv -f UTF-8 -t GBK file_utf8.txt -o file_gbk.txt

# 查看支持的编码
iconv -l
```

## 文件名编码转换

Windows 中文文件名用 GBK，Linux 用 UTF-8，拷贝后文件名可能乱码。

```bash
# 安装 convmv
sudo apt-get install convmv    # Debian/Ubuntu
sudo yum install convmv        # CentOS/RHEL

# 预览转换（不实际操作）
convmv -f UTF-8 -t GBK 文件名

# 实际转换
convmv -f UTF-8 -t GBK --notest 文件名

# 递归处理目录
convmv -r -f GBK -t UTF-8 --notest /path/to/dir/
```

## Vim 推荐编码配置

在 `~/.vimrc` 中添加：

```vim
set encoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,latin1
set fileencoding=utf-8
```

这样 Vim 打开文件时会按顺序尝试编码，优先 UTF-8，最后兜底 latin-1。
