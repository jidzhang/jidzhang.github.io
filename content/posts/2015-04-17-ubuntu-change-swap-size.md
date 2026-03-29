---
title: "Ubuntu修改swap分区的大小"
date: 2015-04-17
draft: false
slug: "ubuntu-change-swap-size"
categories:
  - "Linux"
---

在ubuntu上修改swap分区大小按下面的步骤进行：

    cd  /host/ubuntu/disks/ 

    sudo swapoff  swap.disk 

    sudo rm swap.disk 

    sudo dd if=/dev/zero of=swap.disk bs=1M count=1k   (创建1G的swap, 这步比较慢，1G=bs * count) 

    sudo mkswap -f  swap.disk 

    sudo swapon /host/ubuntu/disks/swap.disk 

最后运行free命令，可以看到swap分区已经改成1G。
