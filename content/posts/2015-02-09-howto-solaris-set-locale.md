---
title: "Solaris 设置 Locale（语言/区域环境）"
date: 2015-02-09
draft: false
slug: "howto-solaris-set-locale"
categories:
  - "Solaris"
---

## 什么是 Locale

Locale 定义了系统的语言和区域习惯，包括字符编码、日期格式、货币符号等。

常用变量：

| 变量 | 作用 |
|------|------|
| `LC_CTYPE` | 字符分类与转换（影响能否显示/输入中文） |
| `LC_MESSAGES` | 程序提示信息的语言 |
| `LC_TIME` | 日期时间格式 |
| `LC_NUMERIC` | 数字格式 |
| `LC_COLLATE` | 字符串排序规则 |
| `LC_ALL` | 覆盖所有 `LC_*` 变量 |
| `LANG` | 默认值，当 `LC_*` 未设置时生效 |

优先级：`LC_ALL` > `LC_*` > `LANG`

## 操作命令

### 查看当前 locale

```bash
locale
```

### 查看已安装的语言包

```bash
locale -a
```

## 设置方法

### 临时生效（当前会话）

```bash
# sh / ksh / bash
LANG=zh_CN.UTF-8; export LANG
LC_ALL=zh_CN.UTF-8; export LC_ALL

# csh
setenv LANG zh_CN.UTF-8
setenv LC_ALL zh_CN.UTF-8
```

### 永久生效（单用户）

在 `~/.profile` 或 `~/.cshrc` 中添加上述 export/setenv 命令。

### 永久生效（全局）

编辑 `/etc/default/init`：

```bash
# 格式：VAR=value，支持 TZ、LANG、LC_*
LANG=zh_CN.UTF-8
LC_ALL=zh_CN.UTF-8
```

修改后重启生效。

---

*参考：[IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/)*
