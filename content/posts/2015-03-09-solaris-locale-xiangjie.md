---
title: "Linux Locale 详解：从原理到配置"
date: 2015-03-09
draft: false
slug: "solaris-locale-xiangjie"
categories:
  - "Linux"
---

> Locale 是 Linux 国际化（i18n）的核心概念。理解它，是解决中文显示、输入问题的基础。

---

## 一、为什么需要设定 Locale

**设定 Locale 与浏览中文网页无关。** 即使 Locale 设为 `en_US.ISO-8859-1`，只要有合适的字体，浏览器照样能显示中文——浏览器自己会检测网页编码并选择对应的字体渲染。

**但 Locale 与"写中文"直接相关。** 如果你需要在终端输入中文、让程序识别中文作为有效字符、让系统消息显示为中文，就必须设定正确的 Locale。

简单说：**看中文靠字体，写中文靠 Locale。**

---

## 二、什么是 Locale

Locale 是根据用户的语言、国家/地区和文化传统定义的**软件运行时语言环境**。它涵盖了日常使用的方方面面：

| 类别 | 变量 | 控制内容 |
|------|------|---------|
| 字符分类 | `LC_CTYPE` | 哪些字符是合法的、大小写转换、标点分类 |
| 排序规则 | `LC_COLLATE` | 字符串比较和排序顺序 |
| 消息语言 | `LC_MESSAGES` | 提示信息、错误信息、菜单的语言 |
| 时间格式 | `LC_TIME` | 日期时间的显示格式 |
| 数字格式 | `LC_NUMERIC` | 小数点、千分位分隔符 |
| 货币格式 | `LC_MONETARY` | 货币符号和格式 |
| 姓名格式 | `LC_NAME` | 姓名书写方式 |
| 地址格式 | `LC_ADDRESS` | 地址书写方式 |
| 电话格式 | `LC_TELEPHONE` | 电话号码格式 |
| 度量衡 | `LC_MEASUREMENT` | 公制/英制 |
| 纸张大小 | `LC_PAPER` | A4 / Letter 等 |
| 概述 | `LC_IDENTIFICATION` | Locale 自身的元数据 |

Locale 定义文件存放在 `/usr/share/i18n/locales/`，如 `zh_CN`、`en_US` 等。

---

## 三、什么是字符集

**Unicode** 是一个静态的编号表，为已知所有符号分配唯一编号（如 U59D0 = "姐"）。

**字符集（Charset/Codeset）** 是这些编号的编码方式——同一个字符在传输时用几个字节表示：

| 字符集 | 编码方式 |
|--------|---------|
| UTF-8 | 拉丁字母 1 字节，常用中文 3 字节，罕见字符 4 字节 |
| GB2312 | 所有字符 2 字节，仅含简体中文 |
| GBK | GB2312 超集，2 字节，含繁体 |
| GB18030 | 变长编码（1/2/4 字节），中国国家标准 |
| ISO-8859-1 | 西欧语言，1 字节，不含中文 |

---

## 四、Locale 名称的含义

格式：`语言[_地域][.字符集][@修正项]`

| Locale | 含义 |
|--------|------|
| `zh_CN.UTF-8` | 中文 + 中国 + UTF-8 编码 |
| `zh_TW.BIG5` | 中文 + 台湾 + Big5 编码 |
| `en_GB.ISO-8859-1` | 英文 + 英国 + ISO-8859-1 |
| `de_DE.UTF-8@euro` | 德语 + 德国 + UTF-8 + 欧洲习惯修正 |

---

## 五、Locale 的优先级

```
LC_ALL > LC_* > LANG
```

- **`LC_ALL`**：最高优先级，覆盖所有其他设置
- **`LC_CTYPE` 等**：单独控制某一类别
- **`LANG`**：默认值，未单独设置的类别都使用 LANG 的值

### 示例

| 设置 | 效果 |
|------|------|
| `LANG=zh_CN.UTF-8` | 所有类别用中文 |
| `LANG=zh_CN.UTF-8` + `LC_MESSAGES=en_US.UTF-8` | 系统消息用英文，其余用中文 |
| `LANG=en_US.UTF-8` + `LC_CTYPE=zh_CN.UTF-8` | 字符分类用中文（可输入中文），其余用英文 |
| `LC_ALL=zh_CN.UTF-8` | 强制所有类别用中文，忽略 LANG 和 LC_* |

---

## 六、中文输入的关键：LC_CTYPE

`LC_CTYPE` 决定了系统内哪些字符是"合法"的。中文 Locale（如 `zh_CN`）会在 `LC_CTYPE` 中定义 `hanzi`（汉字）类别，让中文字符成为有效字符。

英文 Locale（如 `en_US`）中**没有**定义汉字，因此中文字符不被视为有效输入。

**如果你需要中文输入但保持英文界面**（最常见的开发者需求）：

```bash
export LANG=en_US.UTF-8
export LC_CTYPE=zh_CN.UTF-8
```

这样系统消息是英文，但可以输入和显示中文。

---

## 七、Locale 的设置方法

### 临时设置（当前终端）

```bash
export LANG=zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
```

### 永久设置（单用户）

在 `~/.bashrc` 或 `~/.profile` 中添加 export 命令。

### 永久设置（全局）

编辑 `/etc/default/locale`（Debian/Ubuntu）或 `/etc/locale.conf`（CentOS/RHEL）：

```bash
LANG=zh_CN.UTF-8
```

### 查看与生成 Locale

```bash
# 查看当前 locale
locale

# 查看系统支持的所有 locale
locale -a

# 生成指定 locale（Debian/Ubuntu）
sudo locale-gen zh_CN.UTF-8
sudo update-locale LANG=zh_CN.UTF-8

# 生成指定 locale（RHEL/CentOS）
sudo localectl set-locale LANG=zh_CN.UTF-8
```

---

*参考：[ChinaUnix.net](http://bbs.chinaunix.net/thread-834459-1-1.html)*
