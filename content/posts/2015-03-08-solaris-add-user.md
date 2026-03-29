---
title: "Solaris 创建用户"
date: 2015-03-08
draft: false
slug: "solaris-add-user"
categories:
  - "Linux"
---

(1) mkdir

	mkdir -p /export/home/username

(2) adduser

	useradd -s /usr/bin/bash -d /export/home/username username

(3) set password
	
	passwd username

(4) set home
	
	chown username /export/home/username

(5) check

	vi /etc/passwd
	check the last line

PS: more useradd usage should see : [solaris sysadmin guide](http://docs.oracle.com/cd/E19253-01/817-1985/userconcept-11407/index.html)

and [solaris home directories](http://docs.oracle.com/cd/E19253-01/816-5166/useradd-1m/index.html)
