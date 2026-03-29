---
title: "Firefox切换中文或英文界面"
date: 2015-04-07
draft: false
slug: "firefox-switch-chinese-or-english"
categories:
  - "Geek"
---

安装Firefox的时候因为不喜欢火狐中文版自带的乱七八糟的插件，所以选择安装了英文版的。

现在想切换到中文版，毕竟中文版的字体看着习惯。

为了实现中英文切换，需要从官网上安装语言组件，然后改一个设置，很简单。
 
## 英文转中文

1. 安装中文语言组件

这里以16.0.1为例进行说明，你需要找个自己对应的版本。

`ftp://ftp.mozilla.org/pub/mozilla.org/firefox/releases/16.0.1/win32/xpi/`

在上面找到zh-CN.xpi，点击后安装

2. 改设置

在地址栏内输入`about:config`，确认后搜索 `general.useragent.locale`，改为 `zh-CN`
保存后重启Firefox，现在界面就变成中文的了。

## 中文转英文

跟上面的过程很像，先安装en-US.xpi，然后把 `general.useragent.locale` 改为 `en-US` 即可. 
