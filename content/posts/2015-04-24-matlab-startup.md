---
title: "Matlab 启动后闪退的解决方法（AMD CPU）"
date: 2015-04-24
draft: false
slug: "matlab-startup"
categories:
  - "工具"
---

## 问题

Matlab 安装后启动时窗口一闪而过，立即关闭。这通常发生在 AMD 处理器的电脑上。

## 原因

Matlab 默认使用 Intel CPU 的线性代数库（BLAS），与 AMD CPU 不兼容。

## 解决方法

### 1. 确认对应 DLL 存在

检查 Matlab 安装目录下的 `bin\win32`，确认存在对应的 BLAS 库：

| CPU | DLL 文件 |
|-----|---------|
| AMD Athlon | `atlas_athlon.dll` |
| Intel P3 | `atlas_PIII.dll` |
| Intel P2 | `atlas_PII.dll` |

### 2. 设置环境变量

1. 右键"我的电脑" → 属性 → 高级 → 环境变量
2. 新建系统变量：
   - 变量名：`BLAS_VERSION`
   - 变量值：`c:\matlab7\bin\win32\atlas_athlon.dll`（按实际路径和 CPU 型号修改）
3. 确定，重新启动 Matlab

> BLAS（Basic Linear Algebra Subroutines）是基础线性代数子程序库，Matlab 的矩阵运算依赖它。
