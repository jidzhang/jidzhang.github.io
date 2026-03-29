---
title: "WinGet vs Chocolatey vs Scoop 全面对比指南"
date: 2026-03-29
tags: ["Windows", "包管理器", "WinGet", "Chocolatey", "Scoop"]
categories: ["工具"]
---

# WinGet vs Chocolatey vs Scoop 全面对比指南

> Windows 包管理器深度对比 · 2026年3月

---

## 一、引言

Linux 有 apt/yum，macOS 有 Homebrew，Windows 呢？目前有三个主流选择：微软官方的 **WinGet**、老牌的 **Chocolatey**、轻量的 **Scoop**。本文从设计理念、实际使用、进阶技巧到选型建议，帮你全面了解它们的差异。

---

## 二、一句话认识三者

| 包管理器 | 一句话概括 | 类比 |
|----------|-----------|------|
| **WinGet** | 微软官方的包管理器，Windows 11 预装 | 类似 iPhone 自带 App Store |
| **Chocolatey** | 社区驱动的老牌包管理器，企业方案成熟 | 类似 Red Hat 的 yum，成熟但重 |
| **Scoop** | 为开发者设计的轻量安装器，无需管理员权限 | 类似 macOS 的 Homebrew |

---

## 三、安装与首次体验

### WinGet

Windows 11 已预装。Windows 10 需确认 App Installer 已安装：

```powershell
winget --version
```

如果提示找不到命令，在 PowerShell 中执行：

```powershell
Add-AppxPackage -RegisterByFamilyName -MainPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe
```

### Chocolatey

以**管理员身份**打开 PowerShell，执行：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### Scoop

**普通用户权限**即可，打开 PowerShell：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

> **注意**：在中国大陆网络环境下，如果安装脚本报错，建议先配置代理或使用镜像源（见第八节「网络与代理」）。

---

## 四、基本命令对照

| 操作 | WinGet | Chocolatey | Scoop |
|------|--------|------------|-------|
| 搜索 | `winget search <name>` | `choco search <name>` | `scoop search <name>` |
| 安装 | `winget install <name>` | `choco install <name> -y` | `scoop install <name>` |
| 卸载 | `winget uninstall <name>` | `choco uninstall <name>` | `scoop uninstall <name>` |
| 列出已装 | `winget list` | `choco list --local-only` | `scoop list` |
| 更新全部 | `winget upgrade --all` | `choco upgrade all -y` | `scoop update *` |
| 更新工具本身 | Microsoft Store 自动更新 | `choco upgrade chocolatey` | `scoop update` |
| 查看详情 | `winget show <name>` | `choco info <name>` | `scoop info <name>` |
| 清理 | 无内置命令 | `choco optimize`（需商业版） | `scoop cleanup *` |
| 导出列表 | `winget export -o pkgs.json` | `choco list -r > pkgs.txt` | `scoop export > pkgs.json` |
| 锁定版本 | `winget pin add <name>` | `choco pin add -n <name>` | `scoop hold <name>` |

> **提示**：Chocolatey 安装时默认会等待确认，加 `-y` 跳过。WinGet 和 Scoop 默认不等待确认。

---

## 五、设计理念深度解析

### 5.1 权限模型 — 三者的根本分歧

这是选择哪个工具最核心的考量因素。

**WinGet**：跟随应用安装器的行为。如果应用本身需要管理员权限（如写入 Program Files），就会弹出 UAC 提示。WinGet 本身不要求管理员权限，但它安装的东西可能需要。

**Chocolatey**：默认以管理员身份运行。所有包安装到系统级目录（`C:\ProgramData\chocolatey\`），可以操作注册表、安装系统服务。这意味着它**能做任何事**，但也意味着需要信任它。

**Scoop**：核心设计目标是**永不需要管理员权限**。所有内容安装在用户目录下（`~\scoop\`），不碰注册表，不碰系统目录。如果某个应用必须管理员权限，Scoop 会将其归类到 `nonportable` bucket，并明确标注需要管理员。

**实际影响**：
- 公司电脑**没有管理员权限**？只能选 Scoop（或 WinGet 安装用户级应用）
- 需要安装**驱动、VPN 客户端、杀毒软件**？选 WinGet 或 Chocolatey
- 只是装**开发工具**（git、node、python、7zip）？Scoop 最干净

### 5.2 安装位置与系统影响

| 项目 | WinGet | Chocolatey | Scoop |
|------|--------|------------|-------|
| 默认安装位置 | 应用安装器决定（通常是 Program Files） | `C:\ProgramData\chocolatey\` | `%USERPROFILE%\scoop\` |
| 写入注册表 | 取决于应用 | ✅ 通常会 | ❌ 不会 |
| 写入 Program Files | 取决于应用 | ✅ 通常会 | ❌ 不会 |
| 修改 PATH | 取决于应用 | ✅ 系统级 PATH | ✅ 用户级 PATH（shim 机制） |
| 卸载残留 | 取决于应用 | 可能有残留 | 干净卸载（删除文件夹即可） |

**Scoop 的 Shim 机制详解**：

Scoop 安装应用后，不会把应用的真实路径加入 PATH。而是在 `~\scoop\shims\` 目录下创建一个可执行 shim（转发器），只有这个 shim 目录在 PATH 中。这样做的好处：
- PATH 不会随着安装的应用增多而膨胀
- 卸载时只需删除 shim，不需要清理 PATH
- 多版本应用可以共存，通过 `scoop reset` 切换

### 5.3 包的本质 — 它们安装的是什么？

**WinGet**：调用软件的**原生安装器**（.exe、.msi、.msix）。你通过 WinGet 安装 VS Code，和从官网下载安装包安装的结果完全一样。优点是"所见即所得"，缺点是依赖安装器的行为（可能弹出窗口、写入注册表、安装到奇怪的位置）。

**Chocolatey**：运行**社区编写的 PowerShell 安装脚本**（称为 nupkg 包）。这些脚本通常也是下载并执行原生安装器，但可以添加额外的自动化逻辑（如静默参数、配置文件处理等）。优点是灵活，缺点是依赖社区维护者的水平。

**Scoop**：下载应用的**便携版本**（通常是 .zip 或 .7z），解压到本地目录，创建 shim。不运行任何安装器。优点是干净、可预测，缺点是并非所有软件都有便携版本。

---

## 六、实际使用场景深入

### 6.1 开发环境搭建

**典型需求**：安装 Git、Node.js、Python、7-Zip、VS Code、Docker Desktop

**Scoop 方案**（推荐）：

```powershell
# 添加常用 bucket
scoop bucket add extras

# 安装开发工具
scoop install git nodejs python 7zip vscode

# 安装 aria2 加速下载
scoop install aria2

# 多版本 Node.js 共存
scoop install nodejs-lts    # 安装 LTS 版
scoop install nodejs@20     # 安装指定版本

# 切换版本
scoop reset nodejs-lts      # 切换到 LTS
scoop reset nodejs@20       # 切换到 v20
```

**WinGet 方案**：

```powershell
winget install Git.Git
winget install OpenJS.NodeJS.LTS
winget install Python.Python.3.12
winget install 7zip.7zip
winget install Microsoft.VisualStudioCode
winget install Docker.DockerDesktop
```

**对比**：
- Scoop 安装更快（不运行安装器），多版本管理更方便
- WinGet 安装的是官方版本，与从官网下载完全一致
- Docker Desktop 等需要系统服务的应用，Scoop 不支持，需要用 WinGet 或 Chocolatey

### 6.2 批量安装与环境迁移

**WinGet** — 导出/导入最成熟：

```powershell
# 在旧电脑上导出
winget export -o D:\packages.json

# 在新电脑上导入（会自动安装所有包）
winget import -i D:\packages.json --accept-package-agreements --accept-source-agreements
```

**Scoop** — 手动但可控：

```powershell
# 导出
scoop export > D:\scoop-packages.json

# 在新电脑上，先安装 Scoop，然后逐个安装（无内置批量导入）
# 可以用简单的 PowerShell 循环：
Get-Content D:\scoop-packages.json | ConvertFrom-Json | ForEach-Object { scoop install $_.Name }
```

**Chocolatey** — 类似：

```powershell
# 导出
choco list -r > D:\choco-packages.txt

# 导入
Get-Content D:\choco-packages.txt | ForEach-Object { choco install $_.Split()[0] -y }
```

### 6.3 版本管理

| 需求 | WinGet | Chocolatey | Scoop |
|------|--------|------------|-------|
| 安装指定版本 | `winget install <name> -v <version>` | `choco install <name> --version=<ver>` | `scoop install <name>@<version>` |
| 多版本共存 | ❌ | ⚠️ 有限支持 | ✅ 通过 versions bucket |
| 切换版本 | ❌ | ❌ | ✅ `scoop reset <name>@<ver>` |
| 回滚到旧版本 | 需卸载后重装指定版本 | 需卸载后重装 | `scoop reset <name>@<old-ver>` |

**Scoop 版本管理示例**：

```powershell
# 安装多个版本的 Python
scoop install python@3.11
scoop install python@3.12

# 查看已安装版本
scoop list python

# 切换到 3.11
scoop reset python@3.11

# 验证
python --version
```

### 6.4 创建自己的包

**WinGet**：需要向 winget-pkgs 仓库提交 YAML manifest，通过微软审核。适合软件厂商官方提交，个人创建流程较重。

**Chocolatey**：创建 `.nuspec` 文件和 `tools\chocolateyInstall.ps1` 脚本，可以发布到 chocolatey.org 或私有仓库。文档完善，适合企业内部打包。

**Scoop**：只需一个 JSON manifest 文件（通常不到 20 行），描述下载地址、解压方式和 shim。门槛最低，非常适合打包内部工具：

```json
{
    "version": "1.0.0",
    "description": "My internal tool",
    "homepage": "https://internal.example.com/mytool",
    "url": "https://internal.example.com/mytool-1.0.0.zip",
    "hash": "sha256:xxxxxxxxxxxxxxxxxxxxxxxxx",
    "bin": "mytool.exe",
    "extract_dir": "mytool"
}
```

创建私有 Bucket 只需一个 GitHub 仓库：

```powershell
# 创建私有 bucket
scoop bucket add mytools https://github.com/yourname/scoop-mytools

# 安装私有包
scoop install mytools/mytool
```

---

## 七、企业场景

### WinGet 在企业中的角色

WinGet 与 Windows 生态深度集成：
- 通过 **Intune** 或 **Configuration Manager** 部署应用
- 支持 **DSC（Desired State Configuration）** 定义系统状态
- 适合"合规性优先"的企业环境

### Chocolatey for Business

Chocolatey 的商业版提供企业级功能：
- **Central Management**：Web 界面集中管理所有终端的软件
- **Package Internalizer**：将社区包转为离线内网包，不依赖外部源
- **Package Builder**：自动生成高质量安装包
- **Package Synchronizer**：将已安装软件纳入 Chocolatey 管理
- **商业支持**：SLA 保障、技术支持

适合已有 System Center / Chef / Puppet 基础设施的 IT 团队。

### Scoop 在企业中

Scoop **不适合**作为企业级方案：
- 无集中管理能力
- 无商业支持
- 无审计功能

但在受限环境中（开发者没有管理员权限），Scoop 是唯一可行的选择。

---

## 八、网络与代理

在中国大陆使用这些工具时，网络问题可能影响体验。

### WinGet

```powershell
# 查看当前源
winget source list

# WinGet 使用 Microsoft CDN，通常可以直接访问
# 如需代理，设置系统代理或环境变量
$env:HTTP_PROXY = "http://127.0.0.1:7890"
$env:HTTPS_PROXY = "http://127.0.0.1:7890"
```

### Chocolatey

```powershell
# Chocolatey 默认从 chocolatey.org 下载，国内可直连
# 配置代理
choco config set proxy http://127.0.0.1:7890

# 使用镜像源（如有）
choco source add -n mirror -s https://mirrors.example.com/chocolatey/ --priority 1
```

### Scoop

```powershell
# Scoop 从 GitHub 下载，国内可能不稳定
# 方法 1：配置代理
scoop config proxy 127.0.0.1:7890

# 方法 2：使用 aria2 + 代理加速
scoop install aria2
scoop config aria2-enabled true
```

> **Scoop 的 GitHub 依赖是它在国内使用最大的痛点。** 如果你的网络环境访问 GitHub 不稳定，建议使用 Gitee 镜像或配置代理。

---

## 九、性能与资源占用

| 指标 | WinGet | Chocolatey | Scoop |
|------|--------|------------|-------|
| 安装速度 | 中等（依赖应用安装器） | 较慢（PowerShell 脚本 + 安装器） | **快**（解压即用） |
| 磁盘占用 | 取决于应用 | 中等（ProgramData + 缓存） | **低**（用户目录，可控制） |
| 内存占用 | 低（C++ 原生） | 中等（PowerShell 进程） | 低（PowerShell 脚本） |
| 启动速度 | 快 | 较慢（需加载 PowerShell） | 快 |
| 多线程下载 | ❌ | ❌ | ✅（aria2） |

---

## 十、常见问题

### Q：三者可以同时安装吗？
可以，互不冲突。但建议根据使用场景选择主力工具，避免管理混乱。

### Q：WinGet 能完全替代其他两个吗？
不能。WinGet 不支持多版本共存、无用户级隔离、包数量仍在增长中。但作为系统级应用的统一入口，它的角色不可替代。

### Q：Scoop 能装带 GUI 的应用吗？
能。extras bucket 包含大量 GUI 应用（Chrome、Firefox、Telegram、Notion 等），但它们以便携方式运行，部分功能可能受限（如右键菜单集成、文件关联）。

### Q：哪个工具的包更新最及时？
各不相同，取决于包维护者的活跃度。WinGet 有官方提交，Chocolatey 生态最大，Scoop 社区活跃。建议实际搜索你要安装的包，看哪个工具的版本最新。

### Q：卸载哪个工具最干净？
Scoop 最干净 — 删除 `~\scoop` 文件夹和 PATH 条目即可完全移除，不留任何痕迹。WinGet 和 Chocolatey 会有更多残留。

---

## 十一、选择建议

### 按使用场景

| 场景 | 推荐 | 原因 |
|------|------|------|
| 个人开发者日常使用 | **Scoop** | 干净、快速、多版本管理 |
| 系统级应用（驱动、杀毒、VPN） | **WinGet** | 调用原生安装器，兼容性好 |
| 企业 IT 统一管理 | **Chocolatey for Business** | 集中管理、商业支持、审计 |
| 无管理员权限的公司电脑 | **Scoop** | 唯一不需要管理员的选择 |
| 新机器快速搭建环境 | **WinGet + Scoop** | WinGet 管系统应用，Scoop 管开发工具 |
| DevOps 自动化部署 | **WinGet** 或 **Chocolatey** | 脚本友好，企业集成好 |

### 按用户类型

| 用户类型 | 推荐 |
|----------|------|
| 后端/前端开发者 | Scoop（主力）+ WinGet（系统应用） |
| 运维工程师 | Chocolatey 或 WinGet |
| DevOps 工程师 | WinGet（DSC 集成）或 Chocolatey |
| 普通用户 | WinGet（预装，够用） |
| 企业 IT 管理员 | Chocolatey for Business |
| 受限环境开发者 | Scoop（唯一选择） |

---

## 十二、参考资料

- WinGet 官方文档：https://learn.microsoft.com/en-us/windows/package-manager/winget/
- WinGet GitHub：https://github.com/microsoft/winget-cli
- WinGet 包仓库：https://github.com/microsoft/winget-pkgs
- Chocolatey 官网：https://chocolatey.org/
- Chocolatey 文档：https://docs.chocolatey.org/
- Chocolatey 包仓库：https://community.chocolatey.org/packages
- Scoop GitHub：https://github.com/ScoopInstaller/Scoop
- Scoop Wiki：https://github.com/ScoopInstaller/Scoop/wiki
- Scoop Bucket 搜索：https://scoop.sh/

---

*本文基于官方文档和公开信息整理，力求客观准确。具体功能可能随版本更新而变化，建议查阅最新官方文档。*
