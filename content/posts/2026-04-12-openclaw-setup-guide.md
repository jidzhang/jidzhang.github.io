---
title: "OpenClaw 完整配置教程：从安装到深度定制"
date: 2026-04-12
tags: ["OpenClaw", "AI助手", "配置教程"]
categories: ["技术教程"]
---

# OpenClaw 完整配置教程：从安装到深度定制

> 2026-04-13 修正错误

OpenClaw 是一个开源的 AI 助手网关

## 什么是 OpenClaw

OpenClaw 是一个开源的 AI 助手网关（Gateway），它将大语言模型与你日常使用的聊天平台连接起来。通过一个统一的 Gateway 进程，你可以让 AI 助手同时在线 Telegram、WhatsApp、Discord、飞书、Slack 等多个平台，并赋予它执行命令、浏览网页、生成图片等能力。

**核心架构：** 一个长期运行的 Gateway 进程管理所有消息通道，控制平面客户端（macOS 应用、CLI、Web UI）通过 WebSocket 连接到 Gateway。

---

## 一、安装

### 系统要求

| 项目 | 要求 |
|------|------|
| Node.js | 24（推荐）或 22.14+ |
| 操作系统 | macOS / Linux / Windows（原生 + WSL2） |
| 包管理器 | npm（默认）/ pnpm / bun |

### 推荐安装方式

**macOS / Linux / WSL2：**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell)：**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

安装脚本会自动检测操作系统、安装 Node（如需要）、安装 OpenClaw 并启动引导流程。

### 其他安装方式
```bash
# npm 全局安装
npm install -g openclaw@latest
openclaw onboard --install-daemon

# pnpm
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon

# 从源码
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm ui:build && pnpm build
pnpm link --global
openclaw onboard --install-daemon
```

### 验证安装
```bash
openclaw --version      # 确认 CLI 可用
openclaw doctor         # 检查配置问题
openclaw gateway status # 验证 Gateway 运行状态
```

---

## 二、目录结构

OpenClaw 的所有状态和配置集中在 `~/.openclaw/` 目录下：
```
~/.openclaw/
├── openclaw.json          # 主配置文件（JSON5 格式）
├── .env                   # 全局环境变量
├── workspace/             # Agent 工作区
│   ├── AGENTS.md          # Agent 行为指令
│   ├── SOUL.md            # Agent 人格定义
│   ├── USER.md            # 用户信息
│   ├── IDENTITY.md        # Agent 身份
│   ├── TOOLS.md           # 工具配置笔记
│   ├── MEMORY.md          # 长期记忆
│   ├── DREAMS.md          # 梦境日志（实验性）
│   ├── memory/            # 每日笔记
│   │   └── 2026-04-12.md
│   └── skills/            # 工作区级技能
├── credentials/           # 通道凭证
│   ├── whatsapp/          # WhatsApp 凭证
│   └── *-allowFrom.json   # 配对白名单
├── agents/                # Agent 状态
│   └── main/
│       ├── agent/
│       │   └── auth-profiles.json  # 模型认证
│       └── sessions/      # 会话记录
├── cron/                  # 定时任务
│   └── jobs.json
├── skills/                # 管理级技能覆盖
└── secrets.json           # 文件型密钥（可选）
```

---

## 三、配置文件详解

OpenClaw 使用 `~/.openclaw/openclaw.json` 作为主配置文件，支持 JSON5 格式（允许注释和尾逗号）。如果文件不存在，使用安全默认值。

### 最小配置
```json5
// ~/.openclaw/openclaw.json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

### 编辑配置的四种方式
```bash
# 1. 交互式向导
openclaw onboard       # 完整引导流程
openclaw configure     # 配置向导

# 2. CLI 单行命令
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset plugins.entries.brave.config.webSearch.apiKey

# 3. Web 控制台
# 打开 http://127.0.0.1:18789 使用 Config 标签页

# 4. 直接编辑
# 编辑 ~/.openclaw/openclaw.json，Gateway 会自动热重载
```

### 配置热重载

Gateway 会监视配置文件并自动应用更改，无需手动重启。支持四种重载模式：

| 模式 | 行为 |
|------|------|
| `hybrid`（默认） | 安全变更立即热加载，关键变更自动重启 |
| `hot` | 仅热加载安全变更，需要重启时发出警告 |
| `restart` | 任何变更都重启 Gateway |
| `off` | 禁用文件监视，手动重启生效 |
```json5
{
  gateway: {
    reload: { mode: "hybrid", debounceMs: 300 },
  },
}
```

### 拆分配置文件（$include）
```json5
// ~/.openclaw/openclaw.json
{
  gateway: { port: 18789 },
  agents: { $include: "./agents.json5" },
  broadcast: {
    $include: ["./clients/a.json5", "./clients/b.json5"],
  },
}
```

---

## 四、环境变量

### 全局环境变量

| 变量名 | 用途 |
|--------|------|
| `OPENCLAW_HOME` | 内部路径解析的主目录 |
| `OPENCLAW_STATE_DIR` | 覆盖状态目录 |
| `OPENCLAW_CONFIG_PATH` | 覆盖配置文件路径 |
| `OPENCLAW_GATEWAY_PORT` | Gateway 端口（默认 18789） |
| `OPENCLAW_GATEWAY_TOKEN` | Gateway 认证令牌 |
| `OPENCLAW_GATEWAY_PASSWORD` | Gateway 密码 |

### 配置中的环境变量
```json5
{
  // 内联环境变量
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: { GROQ_API_KEY: "gsk-..." },
  },
}
```

### 环境变量替换

在任何配置字符串值中引用环境变量：
```json5
{
  gateway: { auth: { token: "${OPENCLAW_GATEWAY_TOKEN}" } },
  models: { providers: { custom: { apiKey: "${CUSTOM_API_KEY}" } } },
}
```

### 密钥引用（SecretRef）

支持三种密钥来源：`env`（环境变量）、`file`（文件）、`exec`（命令执行）：
```json5
{
  models: {
    providers: {
      openai: {
        apiKey: { source: "env", provider: "default", id: "OPENAI_API_KEY" },
      },
    },
  },
}
```

---

## 五、AI 模型提供商配置

OpenClaw 内置 35+ 模型提供商，只需设置 API 密钥即可使用。

### 设置主模型
```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["openai/gpt-5.4"],
      },
    },
  },
}
```

### 国内模型提供商（重点推荐）

> 如果你在中国大陆使用，以下模型可以直接访问，无需翻墙。OpenClaw 对国内模型的支持非常完善。

#### 智谱 GLM（Z.AI）

智谱 AI 的 GLM 系列是国内最成熟的 AI 模型之一，OpenClaw 原生支持。
```bash
# 国内区域（推荐）
openclaw onboard --auth-choice zai-coding-cn

# 或通用 API Key 设置
openclaw onboard --auth-choice zai-api-key
json5
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

| 模型 | 说明 |
|------|------|
| `zai/glm-5.1` | 最新旗舰，推荐 |
| `zai/glm-5` | 旗舰版 |
| `zai/glm-5-turbo` | 旗舰加速版 |
| `zai/glm-5v-turbo` | 多模态版 |
| `zai/glm-4.7` | 稳定版 |
| `zai/glm-4.7-flash` | 快速版 |
| `zai/glm-4.6` | 经典版 |

API Key 获取：[open.bigmodel.cn](https://open.bigmodel.cn)

#### DeepSeek

DeepSeek 提供高性价比的 AI 模型，API 兼容 OpenAI 格式。
```bash
openclaw onboard --auth-choice deepseek-api-key
json5
{
  env: { DEEPSEEK_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "deepseek/deepseek-chat" } } },
}
```

| 模型 | 上下文 | 最大输出 | 说明 |
|------|--------|----------|------|
| `deepseek/deepseek-chat` | 131K | 8K | DeepSeek V3.2，默认 |
| `deepseek/deepseek-reasoner` | 131K | 65K | 推理增强版 |

API Key 获取：[platform.deepseek.com/api_keys](https://platform.deepseek.com/api_keys)

#### Kimi（Moonshot / 月之暗面）

Kimi K2 系列支持 256K 超长上下文，且当前免费（cost 标记为 0）。
```bash
# 国内端点
openclaw onboard --auth-choice moonshot-api-key-cn

# Kimi Coding（独立提供商，密钥不通用）
openclaw onboard --auth-choice kimi-code-api-key
json5
{
  env: { MOONSHOT_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "moonshot/kimi-k2.5" } } },
}
```

| 模型 | 上下文 | 最大输出 | 说明 |
|------|--------|----------|------|
| `moonshot/kimi-k2.5` | 262K | 262K | 最新旗舰，免费 |
| `moonshot/kimi-k2-thinking` | 262K | 262K | 推理版，免费 |
| `moonshot/kimi-k2-thinking-turbo` | 262K | 262K | 推理加速版 |
| `moonshot/kimi-k2-turbo` | 256K | 16K | 快速版 |

> ⚠️ Moonshot 和 Kimi Coding 是**独立的提供商**，API Key 不通用。Moonshot 用 `moonshot/...`，Kimi Coding 用 `kimi/...`。

API Key 获取：[platform.moonshot.cn](https://platform.moonshot.cn)

#### MiniMax

MiniMax M2.7 支持推理模式，提供 CN 端点。还内置图像生成、音乐生成、视频生成、网页搜索等功能。
```bash
# 国内 OAuth（Coding Plan，推荐）
openclaw onboard --auth-choice minimax-cn-oauth

# 国内 API Key
openclaw onboard --auth-choice minimax-cn-api
```

| 模型 | 上下文 | 最大输出 | 说明 |
|------|--------|----------|------|
| `minimax/MiniMax-M2.7` | 204K | 131K | 推理旗舰 |
| `minimax/MiniMax-M2.7-highspeed` | 204K | 131K | 高速版 |

API Key 获取：[platform.minimax.io](https://platform.minimax.io)

#### 火山引擎（豆包 / Volcengine）

火山引擎提供豆包模型和第三方模型托管，支持通用和编码两种端点。
```bash
openclaw onboard --auth-choice volcengine-api-key
json5
{
  env: { VOLCANO_ENGINE_API_KEY: "..." },
  agents: { defaults: { model: { primary: "volcengine-plan/ark-code-latest" } } },
}
```

| 模型 | 上下文 | 说明 |
|------|--------|------|
| `volcengine/doubao-seed-1-8-251228` | 256K | 豆包 Seed 1.8 |
| `volcengine/kimi-k2-5-260127` | 256K | Kimi K2.5（火山托管） |
| `volcengine/glm-4-7-251222` | 200K | GLM 4.7（火山托管） |
| `volcengine/deepseek-v3-2-251201` | 128K | DeepSeek V3.2（火山托管） |
| `volcengine-plan/ark-code-latest` | 256K | 编码计划默认 |

> 火山引擎的通用端点和编码端点使用同一个 API Key，设置时自动注册两个。

API Key 获取：[console.volcengine.com](https://console.volcengine.com)

#### 阿里云（通义千问 / Qwen）
```bash
openclaw onboard --auth-choice qwen-standard-api-key
```

支持 Qwen 系列文本模型和 Wan 系列视频生成模型。API Key 获取：[dashscope.console.aliyun.com](https://dashscope.console.aliyun.com)

#### 国内模型快速对比

| 模型 | 提供商 | 上下文 | 免费额度 | 特色 |
|------|--------|--------|----------|------|
| GLM-5.1 | 智谱 | 128K | 有 | 原生支持，模型丰富 |
| DeepSeek V3.2 | DeepSeek | 131K | 有 | 高性价比 |
| Kimi K2.5 | 月之暗面 | 262K | ✅ 完全免费 | 超长上下文 |
| MiniMax M2.7 | MiniMax | 204K | 有 | 推理+多模态+音乐 |
| 豆包 Seed 1.8 | 火山引擎 | 256K | 有 | 第三方模型托管 |
| 通义千问 | 阿里云 | 128K+ | 有 | 视频生成 |

### 其他提供商

| 提供商 | 认证变量 | 示例模型 |
|--------|----------|----------|
| OpenAI | `OPENAI_API_KEY` | `openai/gpt-5.4` |
| Anthropic | `ANTHROPIC_API_KEY` | `anthropic/claude-opus-4-6` |
| Google Gemini | `GEMINI_API_KEY` | `google/gemini-3.1-pro-preview` |
| OpenRouter | `OPENROUTER_API_KEY` | `openrouter/auto` |
| Ollama | 无需密钥（本地） | `ollama/llama3.3` |

### API 密钥轮换

支持多个 API 密钥自动轮换（速率限制时切换）：
```bash
# 环境变量设置
OPENAI_API_KEYS="sk-xxx,sk-yyy,sk-zzz"
OPENAI_API_KEY_1="sk-xxx"
OPENAI_API_KEY_2="sk-yyy"
```

### 自定义/自托管提供商
```json5
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        apiKey: "LMSTUDIO_KEY",
        api: "openai-completions",
        models: [
          {
            id: "my-local-model",
            name: "Local Model",
            contextWindow: 200000,
            maxTokens: 8192,
          },
        ],
      },
    },
  },
}
```

### CLI 操作
```bash
openclaw onboard --auth-choice openai-api-key  # 交互式设置
openclaw models set anthropic/claude-opus-4-6  # 设置主模型
openclaw models list                           # 列出可用模型
openclaw models status --probe                 # 检查认证状态
```

---

## 六、消息通道（Channels）

OpenClaw 支持同时连接多个聊天平台，所有通道通过统一 Gateway 管理。

### 内置通道

| 通道 | 说明 | 配置键 |
|------|------|--------|
| Discord | Bot API + Gateway | `channels.discord` |
| Telegram | Bot API via grammY | `channels.telegram` |
| WhatsApp | Baileys（需 QR 配对） | `channels.whatsapp` |
| Signal | signal-cli | `channels.signal` |
| Slack | Bolt SDK | `channels.slack` |
| iMessage | BlueBubbles（推荐） | `channels.bluebubbles` |
| IRC | 标准 IRC | `channels.irc` |
| WebChat | 内置 Web 界面 | 通过 Gateway 访问 |

### 国内聊天平台（重点推荐）

> 如果你在中国大陆使用，以下平台无需翻墙即可使用。

#### 飞书（Lark）

飞书是 OpenClaw 内置支持的通道，使用 WebSocket 长连接，**无需公网 IP 或 Webhook**。

**创建飞书应用：**

1. 访问 [飞书开放平台](https://open.feishu.cn/app)，创建企业自建应用
2. 在 **凭证与基础信息** 中复制 App ID 和 App Secret
3. 在 **权限管理** 中点击批量导入（权限列表见官方文档）
4. 在 **应用能力** > **机器人** 中启用机器人能力
5. 在 **事件订阅** 中选择 **使用长连接接收事件**，添加事件 `im.message.receive_v1`
6. 发布应用版本，等待审批

**配置 OpenClaw：**
```bash
# 交互式配置
openclaw channels add
# 选择 Feishu，输入 App ID 和 App Secret
json5
{
  channels: {
    feishu: {
      enabled: true,
      dmPolicy: "pairing",
      accounts: {
        main: {
          appId: "cli_xxx",
          appSecret: "xxx",
          name: "我的 AI 助手",
        },
      },
    },
  },
}
```

**群聊配置：**
```json5
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["oc_xxx"],  // 群组 ID
      groups: {
        oc_xxx: {
          allowFrom: ["ou_user1"],  // 允许的用户 ID
        },
      },
    },
  },
}
```

飞书支持：文本、富文本、图片、文件、音频、视频、表情、**流式回复**（交互卡片）、文档评论触发、ACP 编码会话、多 Agent 路由。

#### QQ Bot

QQ Bot 通过官方 API（WebSocket 网关）连接，支持私聊、群聊 @消息、频道消息。

**创建 QQ 机器人：**

1. 访问 [QQ 开放平台](https://q.qq.com/)，手机 QQ 扫码登录
2. 点击 **创建机器人**
3. 在设置页复制 **AppID** 和 **AppSecret**

**配置 OpenClaw：**
```bash
openclaw channels add --channel qqbot --token "AppID:AppSecret"
json5
{
  channels: {
    qqbot: {
      enabled: true,
      appId: "YOUR_APP_ID",
      clientSecret: "YOUR_APP_SECRET",
    },
  },
}
```

QQ Bot 支持：文本、图片、语音（STT/TTS）、视频、文件。内置命令：`/bot-ping`、`/bot-version`、`/bot-help`、`/bot-logs`。

#### 企业微信（WeCom）

通过插件支持，需安装社区插件。

#### 微信（WeChat）

通过插件支持，需安装社区插件。

### 通道配置示例
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",
      allowFrom: ["tg:123"],
    },
    discord: {
      enabled: true,
      botToken: "${DISCORD_BOT_TOKEN}",
    },
  },
}
```

### DM 访问策略

| 策略 | 说明 |
|------|------|
| `pairing`（默认） | 未知发送者收到配对码，需审批 |
| `allowlist` | 仅白名单内用户 |
| `open` | 允许所有人（需显式设置 `allowFrom: ["*"]`） |
| `disabled` | 忽略所有 DM |

### 群组提及门控
```json5
{
  channels: {
    whatsapp: {
      groups: { "*": { requireMention: true } },
    },
  },
  agents: {
    list: [{
      id: "main",
      groupChat: { mentionPatterns: ["@openclaw", "openclaw"] },
    }],
  },
}
```

### 配对管理
```bash
openclaw channels login                          # WhatsApp Web 登录
openclaw pairing list whatsapp                   # 查看待审批配对
openclaw pairing approve whatsapp ABC123         # 审批配对码
```

---

## 七、工具集

### 工具配置概览
```json5
{
  tools: {
    profile: "coding",   // messaging | coding | full
    deny: ["group:runtime"],
    fs: { workspaceOnly: true },
    exec: {
      security: "allowlist",  // deny | allowlist | full
      ask: "on-miss",         // off | on-miss | always
    },
    browser: { enabled: true },
  },
}
```

### 工具配置文件（tools.profile）

| 配置 | 说明 |
|------|------|
| `messaging` | 仅消息工具，最安全 |
| `coding` | 编码相关工具（新安装默认） |
| `full` | 所有工具，完全信任 |

### 内置工具一览

| 工具 | 说明 |
|------|------|
| `exec` | 执行 Shell 命令 |
| `read/write/edit` | 文件操作 |
| `browser` | 浏览器自动化 |
| `web_fetch` | 网页抓取 |
| `image_generate` | 图像生成 |
| `video_generate` | 视频生成 |
| `image` | 图像分析 |
| `memory_search/memory_get` | 记忆搜索 |
| `sessions_yield` | 子代理委派 |

### Web 搜索集成

支持 Brave、DuckDuckGo、Exa、Firecrawl、Gemini、Grok、Kimi、MiniMax、Perplexity、SearXNG、Tavily 等搜索引擎。

### 沙箱执行
```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",    // session | agent | shared
      },
    },
  },
}
```

---

## 八、技能系统（Skills）

OpenClaw 使用 AgentSkills 兼容格式的技能文件夹来教 Agent 如何使用工具。

### 技能加载优先级
```
<workspace>/skills（最高）→ <workspace>/.agents/skills → ~/.agents/skills
→ ~/.openclaw/skills → bundled skills → skills.load.extraDirs（最低）
```

### 技能格式

每个技能是一个包含 `SKILL.md` 的文件夹：
```markdown
---
name: image-lab
description: 通过提供商支持的图像工作流生成或编辑图像
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["GEMINI_API_KEY"] },
        "primaryEnv": "GEMINI_API_KEY",
      },
  }
---

# 图像实验室技能指令
使用 `{baseDir}` 引用技能文件夹路径...
```

### 技能配置
```json5
{
  skills: {
    entries: {
      "image-lab": {
        enabled: true,
        apiKey: "YOUR_KEY_HERE",
        env: { GEMINI_API_KEY: "GEMINI_KEY_HERE" },
      },
    },
  },
}
```

### Agent 技能白名单
```json5
{
  agents: {
    defaults: { skills: ["github", "weather"] },
    list: [
      { id: "writer" },                          // 继承默认
      { id: "docs", skills: ["docs-search"] },    // 替换默认
      { id: "locked-down", skills: [] },           // 无技能
    ],
  },
}
```

### ClawHub 技能市场
```bash
openclaw skills search "image"           # 搜索技能
openclaw skills install <skill-slug>     # 安装技能
openclaw skills update --all             # 更新所有技能
openclaw skills list                     # 列出已安装技能
```

---

## 九、记忆系统

OpenClaw 通过纯 Markdown 文件实现跨会话记忆。

### 记忆文件

| 文件 | 用途 | 加载时机 |
|------|------|----------|
| `MEMORY.md` | 长期记忆：持久事实、偏好、决策 | 每个 DM 会话开始时 |
| `memory/YYYY-MM-DD.md` | 每日笔记：运行上下文和观察 | 自动加载今天和昨天的 |
| `DREAMS.md` | 梦境日记（实验性） | 后台整合时 |

### 记忆工具

- **`memory_search`**：语义搜索（混合向量相似度 + 关键词匹配）
- **`memory_get`**：读取特定记忆文件或行范围

### 记忆后端

| 后端 | 说明 |
|------|------|
| Builtin（默认） | SQLite，支持关键词搜索、向量相似度、混合搜索 |
| QMD | 本地优先，支持重排序、查询扩展 |
| Honcho | AI 原生跨会话记忆（插件安装） |

### 记忆搜索配置

当配置了嵌入提供商时，`memory_search` 自动启用混合搜索。OpenClaw 自动检测可用的 API 密钥（OpenAI、Gemini、Voyage、Mistral）。
```bash
openclaw memory status          # 检查索引状态
openclaw memory search "查询"    # 搜索记忆
openclaw memory index --force   # 重建索引
```

### 梦境系统（实验性）

可选的后台记忆整合：收集短期信号、评分候选、将合格条目提升到长期记忆。默认关闭，需手动启用。

---

## 十、定时任务（Cron）

### 配置
```json5
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2,
    sessionRetention: "24h",
    runLog: { maxBytes: "2mb", keepLines: 2000 },
  },
}
```

### CLI 操作
```bash
# 一次性提醒
openclaw cron add \
  --name "提醒" \
  --at "20m" \
  --session main \
  --system-event "检查文档草稿" \
  --wake now

# 定时循环任务
openclaw cron add \
  --name "早报" \
  --cron "0 7 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "总结昨夜更新" \
  --announce \
  --channel telegram \
  --to "123456789"

# 管理任务
openclaw cron list
openclaw cron runs --id <job-id>
openclaw cron run <jobId>
openclaw cron edit <jobId> --message "更新的提示"
openclaw cron remove <jobId>
```

### 调度类型

| 类型 | CLI 参数 | 说明 |
|------|----------|------|
| `at` | `--at` | 一次性时间戳（ISO 8601 或相对如 `20m`） |
| `every` | `--every` | 固定间隔 |
| `cron` | `--cron` | 5/6 字段 cron 表达式 |

### 执行模式

| 模式 | 说明 |
|------|------|
| `main` | 在主会话中执行 |
| `isolated` | 独立会话执行（适合报告、后台任务） |
| `current` | 绑定创建时的会话 |

### Webhook 集成
```json5
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
    mappings: [
      { match: { path: "gmail" }, action: "agent", agentId: "main", deliver: true },
    ],
  },
}
```

---

## 十一、语音配置（TTS）

OpenClaw 支持将文本回复转为语音消息。

### 支持的 TTS 服务

| 服务 | 需要 API Key | 说明 |
|------|-------------|------|
| ElevenLabs | `ELEVENLABS_API_KEY` | 高质量语音合成 |
| OpenAI | `OPENAI_API_KEY` | GPT TTS |
| Microsoft | 不需要 | Edge 神经网络 TTS |
| MiniMax | `MINIMAX_API_KEY` | T2A v2 API |

### 配置示例
```json5
{
  messages: {
    tts: {
      auto: "always",      // off | always | inbound | tagged
      provider: "openai",
      providers: {
        openai: {
          voice: "alloy",
          model: "gpt-4o-mini-tts",
        },
      },
    },
  },
}
```

### Slash 命令
```
/tts on           # 开启
/tts off          # 关闭
/tts status       # 查看状态
/tts provider openai  # 切换提供商
/tts audio 你好    # 一次性语音
```

---

## 十二、安全与审批

### 安全模型

OpenClaw 假设**个人助手**部署模型：每个 Gateway 一个可信操作者。

### 快速安全审计
```bash
openclaw security audit          # 基础审计
openclaw security audit --deep   # 深度审计（含在线探测）
openclaw security audit --fix    # 自动修复常见问题
```

### Gateway 认证
```json5
{
  gateway: {
    bind: "loopback",  // loopback | lan | tailnet | custom
    auth: {
      mode: "token",   // token | password | trusted-proxy
      token: "your-long-random-token",
    },
  },
}
```

### Exec 审批系统

Exec 审批是沙箱 Agent 在真实主机上执行命令的安全护栏：

| 安全级别 | 说明 |
|----------|------|
| `deny` | 阻止所有主机执行 |
| `allowlist` | 仅允许白名单命令 |
| `full` | 允许一切（等于提升权限） |

| 提示模式 | 说明 |
|----------|------|
| `off` | 从不提示 |
| `on-miss` | 白名单未匹配时提示 |
| `always` | 每条命令都提示 |

### 加固基线
```json5
{
  gateway: {
    mode: "local",
    bind: "loopback",
    auth: { mode: "token", token: "replace-with-long-random-token" },
  },
  session: { dmScope: "per-channel-peer" },
  tools: {
    profile: "messaging",
    deny: ["group:automation", "group:runtime", "group:fs"],
    fs: { workspaceOnly: true },
    exec: { security: "deny", ask: "always" },
  },
  channels: {
    whatsapp: { dmPolicy: "pairing", groups: { "*": { requireMention: true } } },
  },
}
```

---

## 十三、多 Agent 路由
```json5
{
  agents: {
    list: [
      { id: "home", default: true, workspace: "~/.openclaw/workspace-home" },
      { id: "work", workspace: "~/.openclaw/workspace-work" },
    ],
  },
  bindings: [
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },
  ],
}
```

每个 Agent 可以有独立的沙箱策略和工具权限：
```json5
{
  agents: {
    list: [
      {
        id: "family",
        workspace: "~/.openclaw/workspace-family",
        sandbox: { mode: "all", scope: "agent", workspaceAccess: "ro" },
        tools: {
          allow: ["read"],
          deny: ["write", "edit", "exec", "browser"],
        },
      },
    ],
  },
}
```

---

## 十四、心跳（Heartbeat）

定期让 Agent 主动检查：
```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",      // 触发间隔
        target: "last",     // last | none | <channel-id>
      },
    },
  },
}
```

---

## 十五、常用命令速查

### 安装与初始化
```bash
openclaw onboard --install-daemon     # 安装并引导
openclaw setup                        # 初始化配置
openclaw doctor                       # 健康检查
openclaw doctor --fix                 # 自动修复
```

### Gateway 管理
```bash
openclaw gateway status               # 查看状态
openclaw gateway start                # 启动
openclaw gateway stop                 # 停止
openclaw gateway restart              # 重启
openclaw dashboard                    # 打开控制台
openclaw logs --follow                # 查看实时日志
```

### 模型管理
```bash
openclaw models list                  # 列出模型
openclaw models set <provider/model>  # 设置主模型
openclaw models status --probe        # 检查认证状态
```

### 通道管理
```bash
openclaw channels list                # 列出通道
openclaw channels status --probe      # 检查通道健康
openclaw channels login               # 登录通道
openclaw pairing list <channel>       # 查看配对请求
openclaw pairing approve <channel> <code>  # 审批配对
```

### 记忆管理
```bash
openclaw memory status                # 记忆索引状态
openclaw memory search "查询"          # 语义搜索
openclaw memory index --force         # 重建索引
```

### 定时任务
```bash
openclaw cron list                    # 列出任务
openclaw cron add --name "任务" ...   # 添加任务
openclaw cron run <jobId>             # 手动触发
openclaw cron remove <jobId>          # 删除任务
```

### 安全
```bash
openclaw security audit               # 安全审计
openclaw security audit --deep        # 深度审计
openclaw security audit --fix         # 自动修复
```

### 技能
```bash
openclaw skills list                  # 列出技能
openclaw skills search "关键词"        # 搜索技能
openclaw skills install <slug>        # 安装技能
openclaw skills update --all          # 更新所有技能
```

### 配置操作
```bash
openclaw config get <path>            # 读取配置
openclaw config set <path> <value>    # 设置配置
openclaw config unset <path>          # 删除配置
openclaw config validate              # 验证配置
openclaw config schema                # 输出 JSON Schema
```

---

## 十六、部署选项

| 方式 | 说明 |
|------|------|
| 本地运行 | macOS 应用 / CLI 前台运行 |
| 系统服务 | macOS LaunchAgent / Linux systemd / Windows 计划任务 |
| Docker | 容器化部署 |
| 云服务器 | VPS / Fly.io / Hetzner / GCP / Azure / Railway |
| Tailscale | 安全远程访问 |

远程访问推荐使用 Tailscale：
```bash
openclaw onboard --tailscale serve   # 通过 Tailscale Serve 暴露
```

---

## 总结

OpenClaw 是一个功能丰富的 AI 助手网关，其核心设计理念是：

1. **统一网关**：一个进程管理所有聊天通道
2. **安全优先**：配对机制、白名单、沙箱、审批系统
3. **灵活配置**：JSON5 配置 + 热重载 + CLI/Web 多种编辑方式
4. **丰富生态**：35+ 模型提供商、20+ 聊天通道、技能市场、插件系统
5. **记忆系统**：基于文件的持久记忆，支持语义搜索
6. **自动化**：定时任务、Webhook、心跳机制

更多信息请访问 [openclaw.ai](https://openclaw.ai) 或查阅官方文档。
