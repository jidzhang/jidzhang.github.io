---
title: "Matlab开启时一闪而过然后自动关闭的解决办法"
date: 2015-04-24
draft: false
slug: "matlab-startup"
categories:
  - ""
---

由于Matlab软件默认的CPU是Intel的，所以AMD处理器的用户在安装 Matlab 后初次运行会自动关闭，可采取以下方式进行解决： 

(1) 假设matlab安装在C盘，确认c:\matlab7\bin\win32下有 atlas_athlon.dll文件 

(2) 在“我的电脑”上点击右键中的“属性”，在“高级”中点“环境 变量”

(3) 在“系统变量”中点“新建”，输入 变量名：BLAS_VERSION 变量值：c:\matlab7\bin\win32\atlas_athlon.dll

另外，顺便要说一下这个BLAS环境变量，这是BasicLinear AlgebraSubroutines的缩写，就是“基础线性几何子程序”的意思。

如果你的CPU是P3的话，要用到C:\Matlab7\bin\win32下的atlas_PIII.dll动态链接库，相应地，P2的话对应atlas_PII.dll，所以设置环境变量的时候要和自己的CPU对应。
