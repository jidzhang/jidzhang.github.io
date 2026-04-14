---
title: "RHEL 将光盘镜像设为 YUM 源"
date: 2015-04-01
draft: false
slug: "rhel-set-yum-repo-to-iso"
categories:
  - "Linux"
---

## 适用场景

RHEL 服务器无法联网，需要从光盘安装软件包。

## 步骤

### 1. 挂载光盘镜像

```bash
mount -o loop rhel-5-server-dvd.iso /media/rhel
```

### 2. 创建 YUM 源配置

编辑 `/etc/yum.repos.d/rhel-local.repo`：

```ini
[Server]
name=Red Hat Enterprise Linux $releasever - Server
baseurl=file:///media/rhel/Server
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[Cluster]
name=Red Hat Enterprise Linux $releasever - Cluster
baseurl=file:///media/rhel/Cluster
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[ClusterStorage]
name=Red Hat Enterprise Linux $releasever - ClusterStorage
baseurl=file:///media/rhel/ClusterStorage
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[VT]
name=Red Hat Enterprise Linux $releasever - VT
baseurl=file:///media/rhel/VT
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```

### 3. 生成 repodata（如需要）

```bash
createrepo -o /var/rhel/Server -g /media/rhel/Server/repodata/comps-rhel5-server-core.xml /media/rhel/Server

# 绑定生成的 repodata
mount --bind /var/rhel/Server/repodata /media/rhel/Server/repodata
```

### 4. 清除缓存

```bash
yum clean all
```

## 验证

```bash
yum list available
```
