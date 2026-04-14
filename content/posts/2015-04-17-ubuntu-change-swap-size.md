---
title: "Ubuntu 修改 Swap 分区大小"
date: 2015-04-17
draft: false
slug: "ubuntu-change-swap-size"
categories:
  - "Linux"
---

## 步骤

```bash
# 1. 关闭当前 swap
sudo swapoff /host/ubuntu/disks/swap.disk

# 2. 删除旧文件
sudo rm /host/ubuntu/disks/swap.disk

# 3. 创建新的 swap 文件（1G = bs × count）
sudo dd if=/dev/zero of=/host/ubuntu/disks/swap.disk bs=1M count=1024

# 4. 格式化为 swap
sudo mkswap -f /host/ubuntu/disks/swap.disk

# 5. 启用
sudo swapon /host/ubuntu/disks/swap.disk
```

## 验证

```bash
free -h
# Swap 行应显示新的大小
```

## 其他大小参考

| 目标大小 | dd 参数 |
|----------|---------|
| 512MB | `bs=1M count=512` |
| 1GB | `bs=1M count=1024` |
| 2GB | `bs=1M count=2048` |
| 4GB | `bs=1M count=4096` |
