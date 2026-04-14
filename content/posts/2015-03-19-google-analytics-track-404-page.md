---
title: "使用 Google Analytics 追踪 404 页面"
date: 2015-03-19
draft: false
slug: "google-analytics-track-404-page"
categories:
  - "Web"
---

## 为什么追踪 404

404 页面意味着用户访问了不存在的 URL。追踪这些请求可以：

- 发现损坏的内部链接
- 了解用户想找什么内容（可能值得补充）
- 找到哪些外部网站链到了你已删除的页面

## 实现方法

在 404 页面的 Google Analytics 追踪代码中，将页面路径改为包含请求 URL 的自定义路径：

**新版 GA（gtag.js）：**

```html
<script>
gtag('config', 'GA_MEASUREMENT_ID', {
  page_path: '/404?page=' + document.location.pathname + document.location.search +
             '&from=' + document.referrer
});
</script>
```

**旧版 GA（analytics.js）：**

```html
<script>
ga('send', 'pageview', '/404?page=' + document.location.pathname +
     document.location.search + '&from=' + document.referrer);
</script>
```

## 查看报告

在 Google Analytics 中查看 **行为 → 网站内容 → 所有页面**，搜索 `/404`，即可看到：

- 哪些 URL 触发了 404
- 来源页面（`from` 参数）
- 访问频率和趋势

根据这些信息修复损坏链接或补充缺失内容。

---

*参考：[月光博客](http://www.williamlong.info/archives/2699.html)*
