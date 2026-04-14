---
title: "Matlab 最小化安装指南"
date: 2015-05-06
draft: false
slug: "matlab-minimal-installer"
categories:
  - "工具"
---

## 问题

Matlab 完整安装动辄 10GB+，对于只需要基本功能的用户来说太臃肿了。

## 最小安装

安装时选择"自定义安装"，只勾选以下组件：

- **MATLAB** — 核心运行环境，必须
- **MATLAB Compiler** — 编译 .m 文件为独立程序
- **Symbolic Math Toolbox** — 符号运算（求导、积分、方程求解等）

其余工具箱（信号处理、图像处理、Simulink 等）按需安装。

## 提示

- 不确定是否需要的组件先不装，后续可通过安装程序追加
- 典型最小安装约 2-3GB
