---
title: "Ubuntu 系统优化三招"
date: 2015-05-07
draft: false
slug: "ubuntu-optimization"
categories:
  - "Linux"
---

## 一、加速 DNS 解析（dnsmasq）

网页打开慢，通常是 DNS 解析耗时。用 dnsmasq 做本地 DNS 缓存：

```bash
# 1. 安装
sudo apt-get install dnsmasq

# 2. 编辑配置
sudo vi /etc/dnsmasq.conf
# 找到 #resolv-file=，改为：
resolv-file=/etc/resolv.dnsmasq.conf

# 3. 复制当前 DNS 配置
sudo cp /etc/resolv.conf /etc/resolv.dnsmasq.conf

# 4. 将系统 DNS 指向本地
sudo vi /etc/resolv.conf
# 去掉原有的 nameserver，添加：
nameserver 127.0.0.1

# 5. 防止 PPPoE 覆盖（如果用拨号上网）
sudo vi /etc/ppp/peers/wvdial
# 在 usepeerdns 前加 #

# 6. 重启系统
sudo reboot
```

## 二、管理启动服务（sysv-rc-conf）

关闭不必要的启动服务，加快开机速度：

```bash
sudo apt-get install sysv-rc-conf
sudo sysv-rc-conf
```

**建议保留的服务：**

| 服务 | 说明 |
|------|------|
| acpid | 电源管理 |
| alsa | 声音 |
| cron | 定时任务 |
| dbus | 消息总线 |
| gdm | 图形登录 |
| klogd | 内核日志 |
| networking | 网络 |
| ssh | 远程登录 |
| sysklogd | 系统日志 |
| udev | 设备管理 |

**可以关闭的：**

| 服务 | 说明 |
|------|------|
| bluetooth | 不用蓝牙就关 |
| ntpdate | 时间同步（按需） |
| cups | 不用打印机就关 |

## 三、优化 Swap 策略

`swappiness` 控制 Linux 使用 swap 的积极程度（0-100）：

```bash
# 查看当前值（默认 60）
sysctl -q vm.swappiness

# 降低 swap 使用倾向（内存够用时）
sudo sysctl vm.swappiness=10

# 永久生效
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
```

| 值 | 含义 |
|------|------|
| 0 | 尽量不用 swap |
| 10 | 桌面推荐 |
| 60 | 默认 |
| 100 | 积极使用 swap |
