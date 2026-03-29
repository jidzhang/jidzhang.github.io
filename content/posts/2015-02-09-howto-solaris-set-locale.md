---
title: "Solaris 设置 locale"
date: 2015-02-09
draft: false
slug: "howto-solaris-set-locale"
categories:
  - "Linux"
---

Locale简单说就是一组「地区语言」的资讯。它包括了

	LC_CTYPE: 字符定义
	LC_MESSAGES: 讯息显示
	LC_TIME: 时间显示格式
	LC_NUMERIC: 数字显示格式
	LC_MONETARY: 货币显示格式
	LC_COLLATE: 字母顺序与字符串比较

其中，与一般使用者最有关系的，是 LC_CTYPE 与 LC_MESSAGES 。

LC_CTYPE 直接关系到某些字符或内码在目前的locale下是否可印? 要如何转换? 对应到那一个字? .... 等等。
LC_MESSAGES 则关系到软体的讯息输出是什么样的语文。真正完整的locale支持，是当我们在shell prompt下，直接设好环境变数，则我们马上就能切换到那个语言。

以上文字由Linux红联社区翻译自：[IBM knowledge centor](http://www-01.ibm.com/support/knowledgecenter/SSKM8N_7.0.0/com.ibm.etools.mft.doc/ae19494_.htm)


## Solaris Locale的设置方法如下

（1）查看当前locale状态：

	# locale
	LANG=en_US
	LC_CTYPE= "en_US"
	LC_NUMERIC= "en_US"
	LC_TIME= "en_US"
	LC_COLLATE= "en_US"
	LC_MONETARY= "en_US"
	LC_MESSAGES= "en_US"
	LC_ALL=en_US

（2）查看系统中已安装的语言包：

	# locale -a

（3）用户自定义locale：

For: sh, ksh, bash

	# LANG=C; export LANG
	# LC_ALL=C; export LC_ALL

For: csh:

	# setenv LANG C
	# setenv LC_ALL

（4）或者编辑环境文件，只对当前用户有效：

	$HOME/.profile or $HOME/.cshrc   

   或，更改系统默认的locale，修改文件：
	
	vi /etc/default/init

example:

	# Lines of this file should be of the form VAR=value, where VAR is one of
	# TZ, LANG, or any of the LC_* environment variables.
	LANG=C
	LC_ALL=C

 

## 附录：

{% highlight bash %}

You can change your system locale on UNIX and Linux systems.
You can set environment variables to control the system locale. You can set these variables to be system-wide, or on a per-session basis:

LC_ALL
    Overrides all LC_* environment variables with the given value
LC_CTYPE
    Character classification and case conversion
LC_COLLATE
    Collation (sort) order
LC_TIME
    Date and time formats
LC_NUMERIC
    Non-monetary numeric formats
LC_MONETARY
    Monetary formats
LC_MESSAGES
    Formats of informative and diagnostic messages, and of interactive responses
LC_PAPER
    Paper size
LC_NAME
    Name formats
LC_ADDRESS
    Address formats and location information
LC_TELEPHONE
    Telephone number formats
LC_MEASUREMENT
    Measurement units (Metric or Other)
LC_IDENTIFICATION
    Metadata about the locale information
LANG
    The default value, which is used when either LC_ALL is not set, or an applicable value for LC_* is not set
NLSPATH
    Delimited list of paths to search for message catalogs
TZ
    Time zone

  LC_MESSAGES and NLSPATH are the most important variables to the broker. These variables define the language and location of response messages that the broker uses. The broker profile file, mqsiprofile, sets NLSPATH. Either you, or your system must set LC_MESSAGES. The value set in LC_MESSAGES must be a value that the broker recognizes. LC_CTYPE is also important to the broker because it defines the character conversion that the broker performs when interacting with the local environment.
   If you use common desktop environment (CDE), use this environment to set the locale instead of setting LANG and LC_ALL directly. The NLSPATH variable respects either method. Before setting the code page, check that it is one of the Supported code pages.
For example, to set WebSphere® Message Broker to run in a UTF-8 environment set the following values in the profile:
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
Where en_US sets the language, and utf-8 sets the code page.

{% endhighlight %}
