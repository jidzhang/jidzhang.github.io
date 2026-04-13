---
title: "openSUSE添加国内的源"
date: 2014-11-13
draft: false
slug: "opensuse-132-repos"
categories:
  - "Linux"
---

### 吐槽
openSUSE的服务器不太给力，经常是十几K，要花很长时间才能下载完程序。
毕竟openSUSE的用户数还没赶上ubuntu。

### 添加国内的源
目前发现的更新及时并且很稳定源是中国科学技术大学的源，用了很久了。  
下面添加的源以13.2为例，换成13.1就可以在openSUSE13.1上用了。
```bash
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/distribution/13.2/repo/oss opensuse-oss
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/distribution/13.2/repo/non-oss opensuse-non-oss
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/update/13.2 opensuse-update
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/update/13.2-non-oss opensuse-update-non-oss
```

