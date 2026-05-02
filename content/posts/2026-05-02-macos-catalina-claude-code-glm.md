---
title: "macOS 10.15 Catalina 上最后的 AI Coding Agent：Claude Code 2.1.112 + 智谱 GLM-5.1"
date: 2026-05-02
tags: ["AI", "Claude Code", "GLM", "macOS", "Coding Agent"]
categories: ["技术实践"]
---

## 背景：老系统上的 Coding Agent 困局

macOS 10.15 Catalina 发布于 2019 年，至今仍有不少开发者在日常使用。然而主流 AI Coding Agent 对系统版本的要求越来越高：

- **OpenAI Codex CLI**：要求 macOS 12+
- **Cursor**：要求 macOS 12+
- **GitHub Copilot**：VS Code 最新版已要求更高系统版本
- **Claude Code**：2.1.112 之后的版本也不再支持 macOS 10.15

这意味着在 macOS 10.15 上，**Claude Code 2.1.112 是目前最后一个可正常运行的 AI Coding Agent**。

## 安装步骤

### 1. 安装 Node.js 20

直接从官网下载安装包，最简单直接：

```
https://nodejs.org/dist/latest-v20.x/node-v20.20.2.pkg
```

下载后双击安装即可。验证：

```bash
node -v   # v20.x.x
npm -v    # 10.x.x
```

**为什么是 Node 20 而不是 22？** Node 22 不支持 macOS 10.15，而 Node 20 LTS 是最后一代支持 Catalina 的版本。Claude Code 2.1.112 在 Node 20 上兼容性也最稳定。

### 2. 配置 npm 镜像源和全局安装目录

macOS 默认的 npm 全局安装路径（`/usr/local`）需要 sudo 权限，容易出问题。两步解决：

**第一步：设置腾讯镜像源**

```bash
npm config set registry https://mirrors.tencent.com/npm/
```

**第二步：设置全局安装目录到用户目录**

```bash
# 创建全局安装目录
mkdir -p ~/.npm-global

# 设置 npm 使用该目录
npm config set prefix ~/.npm-global
```

**第三步：添加到 PATH**

在 `~/.zshrc`（如果用 bash 则是 `~/.bash_profile`）中添加：

```bash
export PATH=~/.npm-global/bin:$PATH
```

然后生效：

```bash
source ~/.zshrc
```

### 3. 安装 Claude Code 2.1.112

```bash
npm install -g @anthropic-ai/claude-code@2.1.112
```

验证：

```bash
claude --version
# 应输出 2.1.112
```

**重要**：不要升级到更高版本。2.1.112 之后的 Claude Code 引入了新的系统依赖，无法在 macOS 10.15 上运行。

### 4. 跳过登录界面

Claude Code 首次启动会要求登录 Anthropic 账号。我们用智谱的 API，不需要登录，所以需要跳过这一步。

编辑 `~/.claude.json`（注意和 `~/.claude/settings.json` 不是同一个文件）：

```json
{
  "hasCompletedOnboarding": true
}
```

这一步必须在配置 API 之前做，否则启动时会卡在登录界面。

## 配置智谱清言 Coding Plan + GLM-5.1

Claude Code 使用 Anthropic 协议通信。智谱提供了一个兼容 Anthropic 协议的端点，可以直接接入。

### 1. 购买 Coding Plan

访问 [智谱开放平台](https://open.bigmodel.cn)，注册账号后在「编码套餐」页面购买。

目前三档：

| 档位 | 月付 | 适合场景 |
|------|------|---------|
| Lite | ¥49 | 轻度使用 |
| Pro | ¥149 | 日常开发 |
| Max | ¥468 | 重度使用 |

所有套餐均支持 GLM-5.1、GLM-5-Turbo、GLM-4.7、GLM-4.5-Air 四个模型。

### 2. 获取 API Key

登录智谱开放平台后，在 [API Keys 页面](https://bigmodel.cn/usercenter/proj-mgmt/apikeys) 创建一个新的 API Key。

### 3. 配置 Claude Code 连接智谱

编辑 `~/.claude/settings.json`：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的智谱API_Key",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-4.7",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-5.1",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-5.1"
  }
}
```

**关键说明：**

- `ANTHROPIC_BASE_URL` 是智谱的 **Anthropic 协议端点**（`/api/anthropic`），不是 OpenAI 格式，也不是通用 API 端点
- `ANTHROPIC_AUTH_TOKEN` 填智谱的 API Key
- 三个模型映射对应 Claude Code 内部的 Opus/Sonnet/Haiku 三个档位，实际调用的是 GLM 模型
- 将 `ANTHROPIC_DEFAULT_OPUS_MODEL` 设为 `glm-5.1` 即可使用 GLM-5.1

### 4. 启动使用

```bash
# 进入项目目录
cd your-project

# 启动 Claude Code
claude
```

启动后如果提示「Do you want to use this API key」，选择 Yes 即可。在 Claude Code 中输入 `/status` 可以确认当前使用的模型。

## 使用技巧

### 用量额度

| 套餐 | 每 5 小时 | 每周 |
|------|----------|------|
| Lite | ~80 次 prompt | ~400 次 |
| Pro | ~400 次 prompt | ~2000 次 |
| Max | ~1600 次 prompt | ~8000 次 |

每次 prompt 预计可调用模型 15-20 次（含多轮内部调用）。

### 专属 MCP 能力

Coding Plan 还附赠视觉理解、联网搜索、网页读取、开源仓库四个 MCP 服务器，可在 Claude Code 中配置使用，进一步增强编码体验。

## 总结

在 macOS 10.15 Catalina 上，Claude Code 2.1.112 是目前唯一可用的主流 AI Coding Agent。配置流程：

1. 安装 Node.js 20.20.2（官网 .pkg 安装包）
2. 设置 npm 腾讯镜像源 + 全局目录 `~/.npm-global`
3. 安装 Claude Code 2.1.112（锁定版本，不升级）
4. 跳过登录（`~/.claude.json` 设 `hasCompletedOnboarding: true`）
5. 配置智谱 Coding Plan 的 Anthropic 协议端点
6. 开始使用

对于不想升级系统的开发者来说，这是目前最务实的选择。
