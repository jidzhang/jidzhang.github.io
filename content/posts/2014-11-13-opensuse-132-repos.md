---
title: "openSUSE 添加国内镜像源"
date: 2014-11-13
draft: false
slug: "opensuse-132-repos"
categories:
  - "Linux"
---

## 问题

openSUSE 官方源在国内下载速度很慢。

## 解决

使用中国科学技术大学镜像源（更新及时、稳定）。

以下以 openSUSE 13.2 为例，其他版本替换版本号即可：

```bash
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/distribution/13.2/repo/oss opensuse-oss
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/distribution/13.2/repo/non-oss opensuse-non-oss
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/update/13.2 opensuse-update
sudo zypper ar -f -c http://mirrors.ustc.edu.cn/opensuse/update/13.2-non-oss opensuse-update-non-oss
```

## 其他镜像

| 镜像站 | 地址 |
|--------|------|
| 中科大 | `mirrors.ustc.edu.cn` |
| 清华 | `mirrors.tuna.tsinghua.edu.cn` |
| 阿里云 | `mirrors.aliyun.com` |
