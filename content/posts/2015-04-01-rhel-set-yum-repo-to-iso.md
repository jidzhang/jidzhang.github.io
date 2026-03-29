---
title: "RHEL 5.x 将光盘指定为YUM服务器"
date: 2015-04-01
draft: false
slug: "rhel-set-yum-repo-to-iso"
categories:
  - "Linux"
---

测试了一下，确实是可以将光盘镜像作为yum的安装服务器。对于喜欢用rhel的人还是比较方便的。理论上也可以将在新版本的rhel出来后，用类似方法yum upgrade实现版本更新。

(1) mount -o loop rhel-5-server-dvd.iso /media/rhel

(2) vi /etc/yum.repos.d/rhel-local.repo

    [Cluster]
    name=Red Hat Enterprise Linux $releasever - $basearch - Cluster
    baseurl=file:///media/rhel/Cluster
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

    [ClusterStorage]
    name=Red Hat Enterprise Linux $releasever - $basearch - ClusterStorage
    baseurl=file:///media/rhel/ClusterStorage
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

    [Server]
    name=Red Hat Enterprise Linux $releasever - $basearch - Server
    baseurl=file:///media/rhel/Server
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

    [VT]
    name=Red Hat Enterprise Linux $releasever - $basearch - VT
    baseurl=file:///media/rhel/VT
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

(3) mkdir -p /var/rhel/{Cluster,ClusterStorage,Server,VT}

(4) createrepo

    createrepo -o /var/rhel/Cluster -g /media/rhel/Cluster/repodata/comps-rhel5-cluster.xml /media/rhel/Cluster
    createrepo -o /var/rhel/ClusterStorage -g /media/rhel/ClusterStorage/repodata/comps-rhel5-cluster-st.xml /media/rhel/ClusterStorage
    createrepo -o /var/rhel/Server -g /media/rhel/Server/repodata/comps-rhel5-server-core.xml /media/rhel/Server
    createrepo -o /var/rhel/VT -g /media/rhel/VT/repodata/comps-rhel5-vt.xml /media/rhel/VT

(5) mount 

    mount --bind /var/rhel/Cluster/repodata /media/rhel/Cluster/repodata
    mount --bind /var/rhel/ClusterStorage/repodata /media/rhel/ClusterStorage/repodata
    mount --bind /var/rhel/Server/repodata /media/rhel/Server/repodata
    mount --bind /var/rhel/VT/repodata /media/rhel/VT/repodata

(6) yum clean all
