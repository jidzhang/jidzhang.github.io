---
title: "修改 Thunderbird 回复邮件格式"
date: 2015-03-21
draft: false
slug: "change-thunderbird-reply-format"
categories:
  - "Geek"
---

## 问题

Thunderbird 默认回复邮件时，回复内容在引用原文的下方，不符合国内邮件习惯。

## 解决步骤

### 1. 回复内容置于最上方

**中文界面：** 工具 → 选项 → 高级 → 常规 → 配置编辑器 → 搜索 `mail.identity.default.reply_on_top`，将值从 `0` 改为 `1`。

**英文界面：** Edit → Preferences → Advanced → Config Editor → 搜索同上。

### 2. 自定义回复头部格式

安装 **SmartTemplate** 扩展（工具 → 附加组件 → 搜索安装），然后在扩展选项中配置回复模板：

```html
-------- 原始信息 --------
<b>发件人</b>: %from%
<b>日期</b>: %date%
<b>收件人</b>: %to%
<b>抄送</b>: %cc%
<b>主题</b>: %subject%
```

### 3. 常用宏

| 宏 | 含义 |
|------|------|
| `%from%` | 发件人完整信息 |
| `%from(name)%` | 发件人姓名 |
| `%from(mail)%` | 发件人邮箱 |
| `%date%` | 原始邮件日期 |
| `%to%` | 收件人 |
| `%cc%` | 抄送人 |
| `%subject%` | 主题 |
| `%Y%-%m%-%d%` | 年-月-日格式 |

> 用 `{...}` 包裹的内容只在宏有值时显示，例如 `{抄送: %cc%}` 只在有抄送时出现。
