---
title: "PHP 启用 Gzip 压缩加速网页加载"
date: 2015-03-23
draft: false
slug: "php-enable-compression"
categories:
  - "Web"
---

## 为什么启用 Gzip

Gzip 压缩可将 HTML/CSS/JS 等文本内容压缩 60%-80%，显著减少传输量，加快页面加载速度。

## 方法一：WordPress 插件

在插件市场搜索 `gzip`，安装 Gzip Compression 相关插件即可。最简单但会多一个插件。

## 方法二：修改主题 functions.php（无需插件）

在 `wp-content/themes/你的主题/functions.php` 末尾添加：

```php
/* 启用 Gzip 压缩 */
add_action('init', 'enable_gzip');

function enable_gzip() {
    ob_start('ob_gzhandler');
}
```

原理：`ob_gzhandler` 是 PHP 内置的输出缓冲回调函数，会自动检测浏览器是否支持 Gzip 并启用压缩。

## 方法三：Apache 配置（推荐）

如果服务器启用了 `mod_deflate`，直接在 `.htaccess` 中添加：

```apache
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml
    AddOutputFilterByType DEFLATE text/css application/javascript
    AddOutputFilterByType DEFLATE application/json
</IfModule>
```

这是最高效的方式，压缩在 Web 服务器层完成，不经过 PHP。

## 验证效果

- Firefox + Firebug（Network 标签）查看响应头中的 `Content-Encoding: gzip`
- 或使用 [PageSpeed Insights](https://pagespeed.web.dev/) 检测
