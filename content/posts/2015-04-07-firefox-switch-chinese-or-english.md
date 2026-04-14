---
title: "Firefox 切换中英文界面"
date: 2015-04-07
draft: false
slug: "firefox-switch-chinese-or-english"
categories:
  - "Geek"
---

## 英文 → 中文

### 1. 安装中文语言包

访问 Firefox 语言包目录，找到对应版本的 `zh-CN.xpi` 安装：

```
https://ftp.mozilla.org/pub/firefox/releases/版本号/win32/xpi/zh-CN.xpi
```

或直接在 Firefox 附加组件中搜索"Chinese"安装。

### 2. 修改语言设置

地址栏输入 `about:config`，搜索 `general.useragent.locale`，改为 `zh-CN`。

重启 Firefox 即可。

## 中文 → 英文

同理：安装 `en-US.xpi`，将 `general.useragent.locale` 改为 `en-US`。

> 较新版本的 Firefox 可直接在 设置 → 常规 → 语言 中切换，无需手动修改配置。
