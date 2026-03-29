---
title: "PHP 实现网页压缩 — Enable compression"
date: 2015-03-23
draft: false
slug: "php-enable-compression"
categories:
  - "Geek"
---

使用gzip压缩页面，可以使得页面载入的更快，这里提供一个解决办法压缩wordpress型博客，当然平台是LAMP。

其实，有这么两个办法:
安装插件 ： wordpress gzip compress 或 gzipp 以及类似的。（用关键字gzip搜索即可） 手动修改配置文件（无需插件）

本文提供手动修改配置文件实现压缩页面（怎么看效果呢？firefox 浏览器+开发工具firebug[开启net]）

在~/wp-content/themes/YourThemes下找到functions.php文件（我用的是默认主题twentyten，如果你的不是，那么应该查找与此类似的一个功能文件），然后在文件的最后添加下面的代码：

{% highlight php %}
/**
* Plugin Name: WordPress Gzip Compression
* Plugin URI:http://tribulant.com
* Description: Enables gzip-compression if the visitor's browser can handle it. This will speed up your WordPress website drastically and reduces bandwidth usage as well. Uses the PHP ob_gzhandler() callback.
* Version: 1.0
* Author: James Socol
* Author URI:http://jamessocol.com/
*/
add_action('init','ezgz_buffer');

function ezgz_buffer ()
{
    ob_start('ob_gzhandler');
}
{% endhighlight %}

看到上面的注释了吗？这段代码就是从Plugin: WordPress Gzip Compression摘下来的。
