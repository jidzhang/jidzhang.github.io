---
title: "NSSM：把任何程序变成 Windows 服务的利器"
date: 2026-05-22
tags: ["Windows", "服务管理", "工具", "开源"]
categories: ["技术工具"]
---

## 什么是 NSSM？

[NSSM](https://github.com/jidzhang/nssm)（**N**ative **S**ystem **S**ervice **M**anager）是一个轻量级的 Windows 服务管理工具。它的核心能力很简单：**把任何可执行程序注册为 Windows 服务**，让它随系统自动启动、崩溃自动重启、后台稳定运行。

NSSM 最初的全称是 "Non-Sucking Service Manager"（不烂的服务管理器），从 v2.26 起正式更名为 **Native System Service Manager**，更准确地反映了它的定位。

如果你曾经在 Windows 上部署过 Node.js 应用、Python 脚本、Java 服务，或者其他任何没有原生服务支持的工具，你一定遇到过这些问题：

- 没法开机自启
- 用户注销后进程就退出了
- 崩溃了没人管
- 日志不知道往哪里写

NSSM 就是来解决这些问题的。

## 为什么选择 NSSM？

同类工具中，WinSW 也是常见选择，但 NSSM 有两个显著优势：

- **不依赖 VC 运行时**——采用多线程静态链接（/MT）编译，不需要安装 Visual C++ Redistributable，拷贝即用
- **体积极小**——exe 仅约 370KB，相比 WinSW 动辄数 MB，NSSM 几乎不占空间

此外，Windows 自带的 `sc` 命令和 `instsrv/srvany` 也能注册服务，但它们有几个致命缺陷：

- **没有崩溃自动重启**——程序挂了就真挂了
- **没有日志重定向**——输出无处可去
- **没有图形界面**——纯命令行操作，参数容易写错
- **不支持相对路径**——路径写死后挪个位置就废了

NSSM 提供了完整的 GUI 配置界面和丰富的命令行参数，覆盖了服务管理的方方面面。

## 下载安装

NSSM 是单文件绿色工具，无需安装，下载后直接使用。

**下载地址：**

- [nssm-2.26-win64.exe](https://github.com/jidzhang/nssm/releases/download/v2.26/nssm-2.26-win64.exe)（64 位 Windows）
- [nssm-2.26-win32.exe](https://github.com/jidzhang/nssm/releases/download/v2.26/nssm-2.26-win32.exe)（32 位 Windows）

> 建议把 `nssm.exe` 放到一个固定目录，并加入系统 PATH 环境变量，方便在任何位置调用。

## 快速入门

### 1. 安装服务（GUI 方式）

以管理员身份打开命令行，输入：

```cmd
nssm install <服务名>
```

例如，把一个 Node.js 应用注册为服务：

```cmd
nssm install myapp
```

执行后会弹出图形配置界面，有多个选项卡：

**Application（应用程序）**
- **Path**：程序路径，例如 `C:\nodejs\node.exe`
- **Startup directory**：启动目录，例如 `D:\myapp`
- **Arguments**：启动参数，例如 `server.js`

**Details（详情）**
- **Display name**：服务显示名称
- **Description**：服务描述
- **Startup type**：启动类型（自动/手动/禁用）

**Log on（登录）**
- 配置服务运行的身份账户，默认使用 LocalSystem

**I/O（输入输出）**
- **Output (stdout)**：标准输出重定向到文件，例如 `D:\myapp\stdout.log`
- **Error (stderr)**：错误输出重定向到文件，例如 `D:\myapp\stderr.log`

填写完 Application 选项卡的 Path 字段后，点击 **Install service** 即可完成注册。

### 2. 安装服务（命令行方式）

不需要 GUI，一条命令搞定：

```cmd
nssm install myapp "C:\nodejs\node.exe" "server.js"
nssm set myapp AppDirectory "D:\myapp"
nssm set myapp AppStdout "D:\myapp\stdout.log"
nssm set myapp AppStderr "D:\myapp\stderr.log"
nssm set myapp Start SERVICE_AUTO_START
```

### 3. 管理服务

```cmd
# 启动服务
nssm start myapp

# 停止服务
nssm stop myapp

# 重启服务
nssm restart myapp

# 查看服务状态
nssm status myapp

# 编辑服务配置（弹出 GUI）
nssm edit myapp

# 删除服务
nssm remove myapp
# 或跳过确认直接删除
nssm remove myapp confirm
```

### 4. 修改配置参数

任何参数都可以用 `set` 命令修改：

```cmd
nssm set myapp AppParameters "--port 8080 --env production"
nssm set myapp AppEnvironmentExtra "NODE_ENV=production"
nssm set myapp Description "我的应用服务"
```

重置某个参数为默认值：

```cmd
nssm reset myapp AppParameters
```

## 常见使用场景

### 场景一：部署 Node.js Web 应用

```cmd
nssm install webapp "C:\Program Files\nodejs\node.exe"
nssm set webapp AppParameters "index.js"
nssm set webapp AppDirectory "D:\projects\webapp"
nssm set webapp AppStdout "D:\projects\webapp\logs\stdout.log"
nssm set webapp AppStderr "D:\projects\webapp\logs\stderr.log"
nssm set webapp AppRotateFiles 1
nssm set webapp Start SERVICE_AUTO_START
nssm start webapp
```

### 场景二：运行 Python 定时任务脚本

```cmd
nssm install dataworker "C:\Python312\python.exe"
nssm set dataworker AppParameters "worker.py"
nssm set dataworker AppDirectory "D:\scripts"
nssm set dataworker AppStdout "D:\scripts\worker.log"
nssm set dataworker AppStderr "D:\scripts\worker.log"
nssm start dataworker
```

### 场景三：运行 Java Spring Boot 应用

```cmd
nssm install springapp "C:\Program Files\Java\jdk-21\bin\java.exe"
nssm set springapp AppParameters "-jar myapp.jar --spring.profiles.active=prod"
nssm set springapp AppDirectory "D:\java\myapp"
nssm set springapp AppStdout "D:\java\myapp\app.log"
nssm start springapp
```

## 进阶功能

### 崩溃自动重启与节流

NSSM 默认会在程序异常退出时自动重启。重启间隔会逐步增加（指数退避），最长到 4 分钟，避免无限循环消耗 CPU。

```cmd
# 设置退出后的默认动作（Restart / Ignore / Exit / Suicide）
nssm set myapp AppExit Default Restart

# 无条件重启延迟（毫秒）
nssm set myapp AppRestartDelay 5000

# 节流阈值（毫秒），程序运行不到此时间就退出则触发节流
nssm set myapp AppThrottle 1500
```

你还可以针对特定的退出码配置不同的动作：

```cmd
# 退出码 0 时直接退出服务（正常退出不重启）
nssm set myapp AppExit 0 Exit

# 退出码 2 时忽略（服务显示运行中但程序已退出）
nssm set myapp AppExit 2 Ignore
```

### 日志输出与文件轮转

NSSM 可以将 stdout 和 stderr 重定向到文件，并支持文件轮转：

```cmd
# 设置输出文件
nssm set myapp AppStdout "D:\myapp\stdout.log"
nssm set myapp AppStderr "D:\myapp\stderr.log"

# 启用文件轮转
nssm set myapp AppRotateFiles 1

# 文件超过 10MB 时轮转
nssm set myapp AppRotateBytes 10485760

# 文件超过 24 小时未修改时轮转
nssm set myapp AppRotateSeconds 86400

# 服务运行期间在线轮转（增加复杂度和风险）
nssm set myapp AppRotateOnline 1

# 手动触发轮转
nssm rotate myapp
```

轮转后的文件会以时间戳命名，例如 `service-20260522T113939.457.log`。

### 时间戳前缀

可以给每行输出自动加上毫秒精度的时间戳：

```cmd
nssm set myapp AppTimestampLog 1
```

效果：

```
2026-05-22 10:17:09.451 Pipeline main started
2026-05-22 10:17:09.632 Listening on port 8080
```

### 进程优先级和 CPU 亲和性

```cmd
# 设置进程优先级
nssm set myapp AppPriority BELOW_NORMAL_PRIORITY_CLASS

# 限制在特定 CPU 核心上运行（0-2 和 4 号核心）
nssm set myapp AppAffinity 0-2,4
```

### 环境变量

```cmd
# 追加环境变量（保留系统环境变量）
nssm set myapp AppEnvironmentExtra "JAVA_HOME=C:\java\jdk21"

# 也可以用 %PATH% 引用已有变量
nssm set myapp AppEnvironmentExtra "PATH=C:\bin;%PATH%"
```

### 服务依赖

```cmd
# 设置依赖（依赖其他服务先启动）
nssm set myapp DependOnService MpsSvc
```

### 优雅停止

NSSM 停止服务时按优先级尝试四种方法：

1. 发送 Ctrl+C 事件（控制台应用可捕获后优雅退出）
2. 发送 WM_CLOSE 消息（GUI 窗口）
3. 发送 WM_QUIT 消息（线程消息队列）
4. 调用 TerminateProcess() 强制终止

每种方法默认等待 1500ms，可分别配置超时时间：

```cmd
nssm set myapp AppStopMethodConsole 3000
nssm set myapp AppStopMethodWindow 2000
nssm set myapp AppStopMethodThreads 1000
```

### 服务事件钩子

可以配置在服务启动、停止等事件时执行自定义命令：

```cmd
# 服务启动前执行
nssm set myapp PreExec "D:\scripts\pre-start.bat"
```

### 服务查询与管理

```cmd
# 列出 NSSM 管理的所有服务
nssm list

# 导出服务配置
nssm dump myapp

# 查看服务管理的进程
nssm processes myapp
```

## v2.26 版本亮点

这个版本基于原始 NSSM 项目进行了全面升级，包含安全加固、bug 修复和测试体系建设。

### 安全加固

- **DLL 加载安全**：使用 `LOAD_LIBRARY_SEARCH_SYSTEM32` 防止 DLL 劫持攻击，同时保留 Windows 2000 兼容回退
- **CreateProcess 参数修复**：修复了 hook.cpp 中首参数提取的漏洞
- **资源泄漏修复**：覆盖 service、io、registry、event、account、gui 等全部模块

### Bug 修复

- `is_admin` 从 `bool` 改为 `BOOL`，修复通过 `PBOOL` 导致的栈损坏
- 移除 io.cpp 中的静态局部变量，消除多实例间数据损坏风险
- GUI 控件和注册表操作的空指针加固
- `get_full_path()` 从静态缓冲区改为输出参数，避免多实例竞争

### 完整的测试套件

- **Catch2 单元测试**：60 个测试用例，113 个断言
- **Pester 3.4 集成测试**：37 个测试用例（Windows 10 内置 Pester，零依赖）
- `run_all_tests.bat` 一键构建和测试
- `clean.bat` 清理所有构建和测试产物

### 支持相对路径（v2.25 引入）

从 v2.25 起，Application 路径、stdout/stderr 输出文件、Startup directory 都支持相对路径。之前必须使用绝对路径，部署位置变了就得重新配置。

## 注意事项

1. **以管理员身份运行**——所有 NSSM 命令都需要管理员权限
2. **不要移动 nssm.exe**——注册服务后，Windows 服务管理器指向的是 nssm.exe 的路径。如果移动了文件位置，需要修改注册表 `HKLM\System\CurrentControlSet\Services\<服务名>\ImagePath`，或者删除重建服务
3. **事件查看器冲突**——如果打开了 Windows 事件查看器，它会锁定 nssm.exe，导致无法替换文件。升级 NSSM 前请先关闭事件查看器
4. **路径引号**——路径含空格时用引号包裹，参数中需要引号时用 `"""引号内容"""` 的方式

## 从源码构建

- **生产构建**：Visual Studio 2008 + `nssm90.sln`
- **开发构建**：Visual Studio 2019+ + `nssm140.sln`

## 总结

NSSM 是 Windows 下做服务化的最简方案。没有复杂的配置文件，没有臃肿的框架，一个 exe 文件就能把任何程序变成可靠的后台服务。无论是开发环境还是生产部署，都是值得常备的工具。

v2.26 版本通过安全加固和完善的测试体系，让 NSSM 更加可靠，适合在生产环境中放心使用。

**项目地址：** <https://github.com/jidzhang/nssm>

**下载地址：**
- [nssm-2.26-win64.exe](https://github.com/jidzhang/nssm/releases/download/v2.26/nssm-2.26-win64.exe)（64 位）
- [nssm-2.26-win32.exe](https://github.com/jidzhang/nssm/releases/download/v2.26/nssm-2.26-win32.exe)（32 位）
