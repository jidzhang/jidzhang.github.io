---
title: "Firefox 启用或禁用 JavaScript"
date: 2015-04-06
draft: false
slug: "firefox-enable-or-disable-javascript"
categories:
  - "Geek"
---

## 问题

Firefox 23+ 在首选项中移除了 JavaScript 开关。如果 JS 被意外禁用，需要通过隐藏配置修改。

## 解决方法

1. 地址栏输入 `about:config`，点击"接受风险并继续"
2. 搜索 `javascript.enabled`
3. 双击该条目切换 `true`（启用）/ `false`（禁用）

立即生效，无需重启。

> 也可以安装 [NoScript](https://noscript.net/) 扩展，按站点精细控制 JS 权限，比全局开关更实用。
