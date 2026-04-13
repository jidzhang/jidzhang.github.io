---
title: "修改Thunderbird回复格式"
date: 2015-03-21
draft: false
slug: "change-thunderbird-reply-format"
categories:
  - "Geek"
---

Thunderbird回复邮件的格式很不好看，而且还是把回复的内容都放在原文引用的下面，用的很不舒服。

下面是对这个使用问题的解决办法：

（1）回复内容放在最前边

英文界面：Edit->Perferences->Advanced->Config Editor->I'll be careful,I promise->mail.identity.default.reply_on_top 会发现目前的默认值为0，改为1即可。

中文界面：工具->选项->高级->常规->配置编辑器->我保证会小心->mail.identity.default.reply_on_top  将默认值由0为1

（2）安装附加插件：

SmartTemplate的强大之处就是可以直接用html控制邮件的格式，

安装完之后在附加组件里找到它，点“选项”，编辑配置，如下图

![thunderbird conf](/images/thunderbird-format.jpg)

首先点选图中所示的几个选项，然后复制如下代码
```html
-------- <FONT face=Verdana size=3>原始信息</FONT> --------
<FONT face=Verdana size=2><b>发件人</b></FONT>: %from%
<FONT face=Verdana size=2><b>日期</b></FONT>: %date%
<FONT face=Verdana size=2><b>收件人</b></FONT>: %to%
<FONT face=Verdana size=2><b>抄送人</b></FONT>: %cc%
<FONT face=Verdana size=2><b>主题</b></FONT>: %subject%
```

到上图所示的空白处，然后确定即可（完全可以依个人口味进行配置）。

其中，%**%之类的东西是SmartTemplate定义的几个宏，完整宏列表在链接 可以找到，或者见下：
```text
Reserved words - Common ：
Reserved words - Common ：
%ownname%
Own account name

%ownmail%
Own mail address

%Y%
Year (1970...)

%m% / %n%
Month (01..12 / 1..12)

%d% / %e%
Day of month (01..31 / 1..31)

%H% / %k%
Hour (00..23 / 0..23)

%I% / %l%
Hour (01..12 / 1..12)

%M%
Minute (00..59)

%S%
Second (00..59)

%p% / %p(x)%
AM or PM, x=1(a.m.)/x=2(A.M.)/x=3(AM)

%A% / %a%
Locale's weekday name (Sunday..Saturday / Sun..Sat)

%B% / %b%
Locale's month name (January..December / Jan..Dec)

Reserved words - Reply/Forward message

%from%
Author

%from(name)%
Author name

%from(firstname)%
Author firstname

%from(mail)%
Author mail address

%date%
Date string (same as mail header syntax)

%datelocal%
Date string (locale format)

%dateshort%
Date string (such as 2000/01/01 1:23:45)

%date_tz%
Time zone (e.g., +0100)

%to%
Recipients(to)

%to(name)%
Recipients name(to)
(mail address, if name is not available.)

%to(mail)%
Recipients mail address(to)

%cc%
Recipients(cc)

%cc(name)%
Recipients name(cc)
(mail address, if name is not available.)

%cc(mail)%
Recipients mail address(cc)

%subject%
Subject

%........%
An any e-mail header(e.g., %Message-Id%, %Newsgroups%, %X-Mailer%, ...)

Reserved words - Additional syntax

{...%reserved word%...}
Phrase enclosed in "{" and "}" is displayed if reserved word has been replaced. Phrase consists of a reserved word and another words.

For example,

To: %toname%{
Cc: %ccname%}
will be replaced as follows, when ccname is not effective, and

To: foo
will be replaced as follows, when ccname is effective.

To: foo
Cc: boo
%X:=sent%
%A-Za-z% is converted to the time the original message was sent.
%X:=today%
%A-Za-z% is converted to today.

Notes about the template with HTML format

Use HTML Escape Characters
" , & , < , > are need to escape by &quot; , &amp; , &lt; , &gt;. And, you can also use other escape characters.

New-line
You need to use <br> for new-line, when 'Replace new-line with <br>' does not used.
```
