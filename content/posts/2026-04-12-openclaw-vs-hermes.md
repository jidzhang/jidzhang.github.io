---
title: "OpenClaw vs Hermes Agent 深度对比：两种设计哲学的碰撞"
date: 2026-04-12
tags: ["OpenClaw", "Hermes Agent", "AI助手", "对比评测"]
categories: ["技术评测"]
---

# OpenClaw vs Hermes Agent 深度对比：两种设计哲学的碰撞

> 2026-04-13 修正错误

> 2026 年最热门的两个开源 AI Agent 框架，一个以网关架构和工程可靠性见长，一个以研究导向和自学习能力取胜。

## 一、项目概况

**OpenClaw** 由社区驱动开发，采用 Node.js 技术栈，定位为"AI 助手网关"——通过一个长期运行的 Gateway 进程统一管理所有消息通道、工具调用和 Agent 生命周期。它强调的是**工程完备性**：热重载、类型安全协议、精细的安全审批、50+ CLI 子命令，覆盖从安装到运维的全生命周期。

**Hermes Agent** 由 Nous Research（知名 AI 研究组织，Hermes、Nomos、Psyche 等模型背后的团队）于 2026 年 2 月开源，两个月内 GitHub 星标突破 40k。采用 Python 技术栈，定位为"可自我进化的 AI Agent 框架"。它强调的是**研究闭环**：Agent 不仅能执行任务，还能将执行轨迹用于 RL 训练，训练出下一代更好的 tool-calling 模型。这是一个"Agent 用自己的经验让自己变强"的飞轮。

两者都兼容 agentskills.io 开放标准，都支持多消息平台、多模型提供商、技能系统和记忆系统，但解决问题的思路截然不同。

---

## 二、架构：网关 vs 编排引擎

### OpenClaw：集中式网关

OpenClaw 的核心是一个 **Gateway 守护进程**，通过 WebSocket 管理所有连接。CLI、macOS 菜单栏应用、Web UI、移动 Node 都是 Gateway 的客户端。这种架构的优势是：

- **单一管控点**：所有通道状态、Agent 状态、会话数据集中管理
- **热重载**：配置变更自动生效，无需重启（hybrid/hot/restart/off 四种模式）
- **控制平面分离**：管理界面和运行时解耦，支持远程管理

配置格式为 JSON5（`~/.openclaw/openclaw.json`），支持 `$include` 拆分配置文件、环境变量替换、SecretRef 密钥引用。整个配置体系经过严格的 Schema 校验——未知字段或错误类型会导致 Gateway 拒绝启动，杜绝了"配错了但不知道"的问题。

### Hermes Agent：单文件编排引擎

Hermes Agent 的核心是 **AIAgent 类**（`run_agent.py`，约 9200 行），一个平台无关的同步编排引擎。它自己处理 provider 选择、prompt 构建、工具执行、重试、fallback、回调、上下文压缩和会话持久化，不依赖外部调度器。这种"all-in-one"的设计让核心逻辑高度内聚，但单文件 9200 行也意味着修改和维护的门槛较高。

配置格式为 `config.yaml` + `.env`，存储在 `~/.hermes/` 目录下。入口点更丰富：

| 入口点 | 用途 |
|--------|------|
| `hermes` CLI | 交互式终端（完整 TUI：多行编辑、斜杠命令补全、流式工具输出） |
| `hermes gateway` | 消息平台网关 |
| ACP (VS Code/Zed/JetBrains) | IDE 内编程助手 |
| Batch Runner | 批量轨迹生成（RL 训练用） |
| API Server | HTTP API 服务 |

支持三种 API 模式：`chat_completions`（OpenAI 兼容）、`codex_responses`（OpenAI Responses API）、`anthropic`（Anthropic Messages API）。会话存储使用 SQLite + FTS5 全文索引，支持会话谱系追踪（parent/child，压缩后保留血缘关系）。

**插件系统**有三个发现源：`~/.hermes/plugins/`（用户级）、`.hermes/plugins/`（项目级）、pip entry points（系统级）。插件可注册工具、钩子（hooks）和 CLI 命令。两种特殊插件类型——memory providers 和 context engines——采用单选模式，同时只能激活一个。

### 设计哲学差异

**OpenClaw** 把复杂性放在架构层面：Gateway 作为中央协调者，通过类型安全协议和严格校验来保证可靠性。它的设计思路更接近"基础设施"——稳定、可预测、好运维。

**Hermes Agent** 把复杂性放在 Agent 层面：一个庞大的编排引擎自己管理一切。它的设计思路更接近"研究平台"——灵活、可扩展、可追踪每一步推理过程。Batch Runner + Atropos 的组合让它成为唯一一个能将 Agent 使用数据直接反哺模型训练的框架。

---

## 三、消息平台：覆盖广度 vs 深度

### 通道数量

| 分类 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **内置通道** | Discord, Telegram, WhatsApp, Slack, Signal, iMessage, IRC, Google Chat, WebChat（9个） | Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Mattermost, Email, SMS（9个） |
| **插件/扩展通道** | 飞书, Matrix, LINE, Teams, QQ Bot, Zalo, Twitch, Nostr, Mattermost 等（12+） | DingTalk(钉钉), Feishu(飞书), WeCom(企业微信), Weixin(微信), BlueBubbles, Home Assistant, Webhook（7+） |
| **总计** | 20+ | 15+ |

### 关键差异

OpenClaw 在**通道数量上略多**，但两者的真正差异在于对中国生态和特定场景的支持：

- **OpenClaw** 内置飞书（WebSocket 连接）和 QQ Bot 插件，通过社区插件支持微信、企业微信
- **Hermes Agent** 原生内置 DingTalk、Feishu、WeCom、Weixin 四个中国平台，这对国内用户是显著优势

另一个值得注意的差异是 **Home Assistant 集成**：Hermes Agent 内置 HA 工具集，可以直接控制智能家居设备，这是 OpenClaw 目前不具备的。此外 Hermes Agent 支持 **Webhook** 通道，可以接收任意 HTTP 回调作为输入——这意味着你可以让 GitHub、Stripe、任何 webhook 驱动的服务直接触发 Agent。

### 会话管理与重置

两者都支持会话管理，但策略不同：

**OpenClaw** 通过 `HEARTBEAT.md` 实现心跳检查，Agent 可以定期自我唤醒执行检查任务。会话可通过 `/new`、`/reset` 手动重置，或通过配置自动管理。

**Hermes Agent** 的会话重置策略更精细，支持按平台配置：
```
策略        默认值           说明
daily       4:00 AM          每天固定时间重置
idle        1440 分钟        空闲 N 分钟后重置
both        组合             任一条件触发即重置
```

这意味着你可以让 Telegram 会话 4 小时空闲后重置，Discord 会话每天凌晨重置——不同平台可以有不同的生命周期策略。

### DM 安全策略

两者都有完善的 DM 访问控制：

| 策略 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **配对机制** | 配对码审批 | 8字符码配对，1小时 TTL，10分钟限流，5次失败锁定 |
| **白名单** | `allowFrom` 列表 | 支持 |
| **配对行为控制** | — | 可按平台设置 pair/ignore |
| **SSRF 防护** | 默认严格 | 不可禁用，阻止私有网络/环回/云元数据，重定向链逐跳验证 |

Hermes Agent 的配对系统设计更精细——限流和失败锁定机制可以有效防止暴力破解。配对行为可以按平台定制，比如 Telegram 用配对、WhatsApp 静默忽略未授权 DM。

---

## 四、工具系统：内置丰富度 vs 灵活性

### 工具规模

OpenClaw 的工具以**内置 first-class** 的方式提供：`exec`、`read`、`write`、`edit`、`web_fetch`、`image`、`image_generate`、`video_generate`、`memory_search`、`memory_get` 等核心工具直接内置于 Agent 运行时。

Hermes Agent 则提供了 **47 个内置工具、40 个 toolsets**，按功能分为八大类：

| 类别 | 包含工具 |
|------|----------|
| **Web** | web_search（4个后端：firecrawl, duckduckgo, google, searxng）、web_extract |
| **Terminal & Files** | terminal, process, read_file, write_file, patch, search_files |
| **Browser** | browser_navigate, browser_snapshot, browser_vision（5个后端：local, browserbase, browseruse, camofox） |
| **Media** | vision_analyze, image_generate, text_to_speech |
| **Agent 编排** | todo（任务规划）、clarify（向用户提问）、execute_code（沙箱 Python）、delegate_task（子代理） |
| **Memory & Recall** | memory（增删改）、session_search（FTS5） |
| **Automation** | cronjob（7种操作）、send_message（跨平台投递） |
| **Integrations** | Home Assistant 工具集、MCP 服务器工具、RL 训练工具（rl_*） |

### 终端执行

这是两个框架差异最大的地方之一。

**OpenClaw** 的 `exec` 工具采用**三级安全 + 三级提示**体系：

- 安全模式：`deny` / `allowlist` / `full`
- 提示模式：`off` / `on-miss` / `always`
- Safe Bins：`head`、`tail`、`tr` 等纯 stdin 工具无需白名单
- 支持**通道内审批**：通过聊天消息 `/approve` 远程审批命令
- 审批时**绑定具体文件**，防止内容漂移
- 支持 PTY 模式运行需要终端交互的工具（如 Codex、Claude Code）

**Hermes Agent** 提供 **6 种终端后端**：local, docker, ssh, singularity, modal, daytona。其中 Docker 后端有严格的安全加固：
```bash
# Docker 安全标志
--cap-drop ALL              # 丢弃所有 Linux capabilities
--cap-add DAC_OVERRIDE      # root 可写挂载目录
--cap-add CHOWN             # 包管理器需要
--cap-add FOWNER            # 包管理器需要
--security-opt no-new-privileges  # 阻止提权
--pids-limit 256            # 限制进程数
--tmpfs /tmp:rw,nosuid,size=512m
--tmpfs /var/tmp:rw,noexec,nosuid,size=256m
--tmpfs /run:rw,noexec,nosuid,size=64m
```

这意味着 Hermes Agent 的 Docker 执行环境比 OpenClaw 更严格。而且 Docker 容器内默认不传入任何宿主环境变量（需要通过 `docker_forward_env` 或 skill 声明确认式传递），从架构层面防止凭证外泄。

所有容器后端（Docker、Singularity、Modal、Daytona）运行时会**跳过危险命令审批**，因为容器本身就是安全边界。这是一个优雅的设计——当你用容器隔离时，审批就不再需要了。

### 浏览器自动化

Hermes Agent 的浏览器工具支持 **5 个后端**（local, browserbase, browseruse, camofox），远超 OpenClaw 的单一 CDP 控制。特别是 `browseruse` 和 `camofox` 后端，为无头浏览器场景提供了更多选择。

### 后台进程管理

两者都支持后台任务执行：

**OpenClaw** 通过 `exec` 的 `background`/`yieldMs` 参数启动后台进程，用 `process` 工具管理（poll/log/kill/write/send-keys）。

**Hermes Agent** 的 `process` 工具提供了完整的生命周期管理：
```python
# 启动后台进程
terminal(command="pytest -v tests/", background=True)
# 返回: {"session_id": "proc_abc123", "pid": 12345}

# 管理
process(action="list")           # 列出所有进程
process(action="poll")           # 检查状态
process(action="wait")           # 阻塞等待完成
process(action="log")            # 获取完整输出
process(action="kill")           # 终止
process(action="write", data="y")  # 发送输入
```

PTY 模式（`pty=true`）可启动交互式 CLI 工具（如 Codex、Claude Code），支持实时输入输出。

### 网页搜索后端

Hermes Agent 的 `web_search` 支持 4 个后端：**Firecrawl**（默认，高质量）、DuckDuckGo（免费备选）、Google、SearXNG（自建）。这意味着即使没有 Firecrawl API Key，搜索功能也能工作——DuckDuckGo 技能会自动作为 fallback 出现。

OpenClaw 的 `web_fetch` 更侧重页面内容提取，搜索能力相对有限。

---

## 五、技能系统：渐进式披露 vs 门控加载

两者都兼容 agentskills.io 开放标准，使用 `SKILL.md` 格式，但实现策略差异显著。

### OpenClaw：门控加载

- 技能通过 `metadata.openclaw.requires` 声明依赖条件
- 加载优先级：`workspace/skills → workspace/.agents/skills → ~/.agents/skills → ~/.openclaw/skills → bundled → extraDirs`
- Skills Watcher 监视文件变化，自动热刷新
- 技能市场：ClawHub（clawhub.ai），支持 `openclaw skills install <slug>` 一键安装

### Hermes Agent：渐进式披露 + 程序性记忆

Hermes Agent 的技能系统设计更复杂、更有野心：

**三级渐进式披露**解决了技能数量爆炸时的 token 消耗问题：

| 级别 | 操作 | Token 开销 |
|------|------|-----------|
| Level 0 | `skills_list` — 获取技能名称和描述列表 | ~3k tokens |
| Level 1 | `skill_view` — 查看完整技能内容 + 元数据 | 按需 |
| Level 2 | `skill_view(path)` — 读取特定参考文件 | 按需 |

Agent 不需要一次性加载所有技能细节，而是根据任务需要逐级深入。对于一个有 50+ 技能的 Agent 来说，这意味着系统提示词可以始终保持精简。

**程序性记忆（Procedural Memory）** 是 Hermes Agent 最独特的功能：当 Agent 完成一个涉及 5+ 工具调用的复杂任务后，可以**自主创建、修改或删除技能**来保存学到的经验。这不是预设的技能，而是 Agent 自己总结出来的"做事方法"。

触发条件包括：
- 成功完成复杂任务后自动总结
- 走了弯路后记录正确的做法
- 用户纠正了方法后更新
- 发现了非显而易见的工作流

技能修改支持四种粒度：`create`（全新创建）、`patch`（定向修补，更省 token）、`edit`（整体替换）、`delete`（删除）。`patch` 模式特别巧妙——它只传递变更的文本，不需要发送完整技能内容。

**条件激活**让技能只在特定环境下出现：
```yaml
metadata:
  hermes:
    # 只有当 Firecrawl 不可用时才显示 DuckDuckGo 搜索技能
    fallback_for_toolsets: [web]
    # 只有当 Docker 可用时才显示容器管理技能
    requires_toolsets: [docker]
    # 同样支持单工具级别
    fallback_for_tools: [web_search]
    requires_tools: [terminal]
```

**Skills Hub** 集成了 7 个来源：

| 来源 | 说明 | 示例 |
|------|------|------|
| official | Hermes 仓库内的可选技能 | `official/security/1password` |
| skills.sh | Vercel 的公共技能目录 | `skills-sh/vercel-labs/...` |
| well-known | URL 发现协议（/.well-known/skills/） | `well-known:https://mintlify.com/...` |
| github | 直接从 GitHub 安装 | `openai/skills/k8s` |
| clawhub | 第三方技能市场 | `clawhub.ai` |
| claude-marketplace | Claude 兼容的 manifest 仓库 | `anthropics/skills` |
| lobehub | LobeHub 公共 Agent 目录 | `lobehub/lobe-chat-agents` |

所有 hub 技能经过**安全扫描**（数据外泄、prompt injection、破坏性命令、供应链信号）。用户可用 `--force` 覆盖非危险级别的策略阻止，但危险判定不可覆盖。信任等级分为：builtin → official → trusted → community。

**平台限制**：技能可以声明 `platforms: [macos]`，在不兼容的平台上自动隐藏。

**外部技能目录**：支持扫描共享目录（如 `~/.agents/skills/`），只读发现、本地优先。多个 AI 工具可以共享同一套技能。

**技能配置**：技能可以声明非密钥配置项（路径、偏好），存储在 `config.yaml` 中，加载时自动注入上下文。

### 分析

OpenClaw 的技能系统更注重**可控性和运维友好**——你清楚知道有哪些技能、从哪里加载、什么时候生效。Hermes Agent 的技能系统更注重**智能性和自适应性**——Agent 自己决定需要什么技能、甚至自己创造新技能。前者适合生产环境，后者适合探索和研究。Hermes Agent 的 7 个 Skills Hub 来源和 4 级信任体系也比 OpenClaw 的 ClawHub 更成熟。

---

## 六、记忆系统：Markdown 文件 vs 多后端

### OpenClaw：纯文件记忆

OpenClaw 的记忆系统完全基于 Markdown 文件：

- `MEMORY.md` — 长期记忆，手动或心跳时整理
- `memory/YYYY-MM-DD.md` — 每日笔记
- `SOUL.md` — Agent 人格定义
- `USER.md` — 用户信息
- `DREAMS.md` — 梦境日志（实验性，Agent 在空闲时产生的自发思维）

语义搜索通过 `memory_search` 工具实现，支持混合（向量 + 关键词）检索。心跳机制定期唤醒 Agent 整理记忆。

这种方式的好处是**透明、可审计、可手动编辑**。你的记忆就是一堆 .md 文件，随时可以用文本编辑器查看和修改。

### Hermes Agent：多后端 + 智能检索

Hermes Agent 的内置记忆也是文件驱动：`MEMORY.md`（2200 字符/~800 tokens）和 `USER.md`（1375 字符/~500 tokens），但有两个关键区别：

**1. 冻结快照模式**

记忆在会话开始时一次性注入系统提示词，会话期间**不会动态更新**。Agent 在会话中添加/修改的记忆条目立即持久化到磁盘，但要到下一次会话才能出现在系统提示词中。这是有意为之的设计——保持 LLM 前缀缓存（prefix cache）的稳定性。

系统提示词中的记忆块包含使用率信息，帮助 Agent 了解容量状态：
```
══════════════════════════════════════════════
MEMORY (your personal notes) [67% — 1,474/2,200 chars]
══════════════════════════════════════════════
User's project is a Rust web service at ~/code/myapi using Axum + SQLx
§
This machine runs Ubuntu 22.04, has Docker and Podman installed
§
User prefers concise responses, dislikes verbose explanations
```

**2. 严格的容量管理**

| 存储 | 限制 | 典型条目数 |
|------|------|-----------|
| memory | 2200 字符 (~800 tokens) | 8-15 条 |
| user | 1375 字符 (~500 tokens) | 5-10 条 |

当记忆满时，Agent 必须先合并或删除旧条目才能添加新条目。这种限制迫使 Agent 优先保存高价值信息，而不是无限制地堆积。

记忆条目经过**安全扫描**——检测 prompt injection、凭证外泄、SSH 后门等攻击模式，以及不可见 Unicode 字符。重复条目自动拒绝。

**3. Session Search（跨会话检索）**

SQLite FTS5 全文搜索 + Gemini Flash 自动摘要，可以高效检索历史会话。这是记忆系统的重要补充——记忆文件只存关键事实，Session Search 负责查找"上周我们讨论过什么"。

| 特性 | 持久记忆 | Session Search |
|------|----------|---------------|
| 容量 | ~1300 tokens | 无限 |
| 速度 | 即时（在系统提示词中） | 需搜索 + LLM 摘要 |
| 用途 | 关键事实始终可用 | 查找特定历史对话 |
| 管理 | Agent 手动管理 | 自动存储所有会话 |

**4. 8 个外部记忆提供商**

Honcho（辩证式用户建模）、OpenViking、Mem0、Hindsight、Holographic、RetainDB、ByteRover、Supermemory。这些提供商提供知识图谱、语义搜索、自动事实提取、跨会话用户建模等能力，与内置记忆并行运行（不替代）。

### 分析

OpenClaw 的文件记忆**简单透明、零依赖**，适合个人使用和小规模部署。Hermes Agent 的多后端记忆**扩展性强**，适合需要大量历史数据检索的场景。Hermes Agent 的冻结快照 + 容量管理设计更精细，但也意味着记忆更新有延迟。两者各有取舍——透明性 vs 检索能力，即时更新 vs 缓存稳定。

---

## 七、语音能力

### OpenClaw

- TTS：ElevenLabs, OpenAI TTS, Microsoft Edge TTS（免费）, MiniMax
- 特色：模型可为单条回复动态切换语音参数，长文本自动摘要后再转语音，Telegram/WhatsApp 自动使用 Opus 格式

### Hermes Agent

Hermes Agent 的语音系统覆盖了更多场景：

**输入（STT）**：

| 提供商 | 模型 | 速度 | 质量 | 费用 | 需要 Key |
|--------|------|------|------|------|---------|
| Local (faster-whisper) | base/small/medium/large-v3 | 取决于 CPU/GPU | 好 | 免费 | ❌ |
| Groq | whisper-large-v3-turbo | ~0.5s | 好 | 免费额度 | ✅ |
| OpenAI | whisper-1 / gpt-4o-transcribe | ~1s / ~2s | 好 / 最佳 | 付费 | ✅ |

自动 fallback 优先级：local → groq → openai

**输出（TTS）**：

| 提供商 | 质量 | 费用 | 延迟 | 需要 Key |
|--------|------|------|------|---------|
| Edge TTS | 好 | 免费 | ~1s | ❌（322 种声音，74 种语言） |
| ElevenLabs | 优秀 | 付费 | ~2s | ✅ |
| OpenAI TTS | 好 | 付费 | ~1.5s | ✅ |
| NeuTTS（本地） | 好 | 免费 | 取决于硬件 | ❌ |
| MiniMax | 好 | 免费 | — | ✅ |

**独特能力**：
- **CLI 语音模式**：`Ctrl+B` 录制，自动静音检测（两阶段算法：语音确认 + 3 秒静音结束），流式 TTS 逐句回复，自动重启录制形成连续对话
- **Gateway 语音回复**：Telegram/Discord 语音消息，支持 voice_only（仅语音消息触发）和 all（所有消息都回复语音）
- **Discord 语音频道**：bot 加入 VC，实时收听用户发言，处理后用语音回复。支持文本频道同步显示转录内容，自动回声抑制（播放 TTS 时暂停监听），仅允许白名单用户交互
- **幻觉过滤器**：26 个已知 TTS 幻觉短语 + regex 模式（多语言），自动过滤 Whisper 的"Thank you for watching"等误识别
- **流式 TTS**：将文本增量缓冲为完整句子后逐句播放，不等完整响应生成完毕

Hermes Agent 在语音领域的深度明显超过 OpenClaw，特别是 Discord VC 实时语音、本地 STT/TTS 支持和幻觉过滤器。这些功能让 Hermes Agent 可以作为真正的语音助手使用，而不仅仅是偶尔发语音消息。

---

## 八、安全体系

### OpenClaw

- 安全审计 CLI：`openclaw security audit`，支持 `--deep` 和 `--fix`
- Gateway 认证：token / password / trusted-proxy
- Exec 审批：三级安全 + 三级提示
- DM 配对：配对码机制
- 浏览器 SSRF 防护
- 工具配置文件：messaging / coding / full
- SecretRef：env / file / exec 三种密钥来源
- 文件权限检查：审计时自动检查
- 60 秒加固基线：一行命令获得安全配置

### Hermes Agent

Hermes Agent 定义了 **7 层安全架构**：

**1. 用户授权**
- DM 配对系统（8字符码，密码学随机，1小时TTL，10分钟限流，最大3个待审批码/平台，5次失败→1小时锁定）
- 平台级 + 全局级白名单
- 支持 `ALLOW_ALL_USERS` 开放模式

**2. 危险命令审批**
三种模式：

| 模式 | 行为 |
|------|------|
| manual（默认） | 所有危险命令都需要用户确认 |
| smart | 辅助 LLM 评估风险，低风险自动通过，真正危险的自动拒绝，不确定的升级为人工确认 |
| off | 跳过所有检查（仅限受信环境） |

危险模式覆盖了 30+ 种场景：`rm -r`、`chmod 777`、`mkfs`、`dd if=`、`DROP TABLE`、`curl | sh`、进程替换注入、写入 `/etc`、`systemctl stop`、fork 炸弹等。

审批流程（CLI）提供四个选项：once（单次）、session（会话内）、always（永久加入白名单）、deny（拒绝）。Gateway 环境下通过聊天消息回复 yes/no 审批。

YOLO 模式可通过 `--yolo` 参数、`/yolo` 斜杠命令或 `HERMES_YOLO_MODE=1` 环境变量激活。

**3. 容器隔离**
Docker 后端默认不传入宿主环境变量，需要通过 `docker_forward_env`（显式白名单）或 skill 声明的 `required_environment_variables` 确认式传递。凭证文件只读挂载。资源限制可配置（CPU、内存、磁盘）。

**4. MCP 凭证过滤**
MCP stdio 子进程只接收安全系统变量（PATH, HOME, USER 等）+ MCP 配置中显式定义的变量。错误消息中的凭证（GitHub PAT、API Key、Bearer Token）自动替换为 `[REDACTED]`。

**5. 上下文文件扫描**
检测项目文件中的 prompt injection 和数据外泄模式。

**6. 跨会话隔离**
会话间无法访问彼此的数据或状态。Cron 任务的存储路径经过加固，防止路径遍历攻击。

**7. 输入消毒**
终端工具后端的工作目录参数经过白名单验证，防止 shell 注入。

**Tirith 预执行扫描**集成（自动从 GitHub Releases 安装，SHA-256 校验 + cosign 签名验证）：
- 同形字 URL 欺骗检测（国际化域名攻击）
- Pipe-to-interpreter 模式检测
- 终端注入攻击检测

**SSRF 防护**始终启用且不可禁用：
- 私有网络（RFC 1918）：10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- 环回：127.0.0.0/8, ::1
- 链路本地：169.254.0.0/16（含云元数据 169.254.169.254）
- CGNAT/共享地址（RFC 6598）：100.64.0.0/10（Tailscale, WireGuard）
- 云元数据主机名：metadata.google.internal, metadata.goog
- DNS 解析失败视为阻止（fail-closed）
- 重定向链逐跳重新验证

### 分析

OpenClaw 的安全优势在于**运维体验**——`openclaw security audit --fix` 一条命令完成安全加固，SecretRef 三种密钥来源让凭证管理更灵活。Hermes Agent 的安全优势在于**纵深防御**——7 层架构覆盖了更多攻击面，smart 审批模式用 LLM 辅助判断减少了误报，MCP 凭证过滤和 SSRF 防护的设计更严谨。特别是 SSRF 防护的 fail-closed 设计和重定向链逐跳验证，是生产级的安全实践。

---

## 九、上下文管理与压缩

### OpenClaw

OpenClaw 通过 `memory_search` 和 `memory_get` 工具进行按需记忆检索，避免将所有上下文塞入 prompt。心跳机制定期整理记忆文件。支持会话修剪（session pruning）和压缩（compaction）。

### Hermes Agent

Hermes Agent 有更精细的上下文管理：

- **可配置压缩阈值和目标比率**：默认 50% 触发，压缩到 20%
- **独立压缩模型**：可以用便宜的模型（如 Gemini Flash）做压缩，节省主力模型的 token 成本。压缩不需要最强的推理能力，用便宜模型是明智的选择
- **Anthropic Prompt Caching**：原生支持 Anthropic 的 prompt caching 机制，缓存系统提示词中不变的 prefix 部分
- **冻结快照**：系统提示词在会话开始时固定，不因记忆更新而变化，保护 LLM 前缀缓存

三个参数的协作方式：threshold 控制何时开始压缩，target_ratio 控制压缩到什么程度。threshold 越低、target_ratio 越小 = 压缩越激进。配合独立压缩模型，可以实现"用最少的钱维持最长的对话"。

### 分析

Hermes Agent 在上下文管理上的设计更精细，特别是独立压缩模型和 prompt caching 的组合。OpenClaw 的会话修剪更自动化，用户感知更少。

---

## 十、子代理与委托

### OpenClaw

- 通过 `sessions_spawn` 生成子代理，支持 `subagent` 和 `acp`（编码代理）两种运行时
- 编码技能可委托给 Codex、Claude Code、Pi 等外部编码代理
- 子代理结果通过 push 机制自动返回
- 支持后台持久会话（`mode="session"`）

### Hermes Agent

- 隔离子代理用于并行工作流，支持自定义模型/提供商覆盖
- **`execute_code` 上下文折叠**：将多步骤 Python 管道折叠为单次推理调用，复杂操作不消耗主 Agent 的上下文窗口。这是一个非常实用的优化——想象一下一个需要 10 步的数据处理管道，如果每步都经过主 Agent 的上下文，会消耗大量 token。execute_code 将它变成一次调用
- **PTY 模式**支持交互式 CLI 工具（如 Codex、Claude Code），可以与外部编码代理实时交互
- 委托模型配置优先级：`delegation.base_url > delegation.provider > 继承父级`

### 分析

OpenClaw 的子代理系统与 ACP 集成更紧密（原生支持 VS Code、Claude Code 等编码代理），Hermes Agent 的 execute_code 上下文折叠是更优雅的 token 优化方案。

---

## 十一、RL/研究集成（Hermes Agent 独有）

这是 Hermes Agent 最核心的差异化特性，也是 OpenClaw 完全不具备的能力。

**Atropos 集成**允许 Hermes Agent 将任务执行轨迹转化为 RL 训练数据：

1. Agent 执行任务，记录完整的推理和工具调用轨迹
2. Batch Runner 批量生成 ShareGPT 格式的 trajectory
3. 轨迹输入 Atropos RL 训练环境
4. 训练出下一代更好的 tool-calling 模型
5. 新模型部署回 Agent，形成**自学习闭环**

代码结构中 `environments/` 目录包含了完整的 RL 框架：Atropos 环境、多工具调用解析器、trajectory 生成器。这意味着 Hermes Agent 不仅仅是一个"使用 AI 模型的工具"，它是一个**生产 AI 模型的平台**。Agent 的使用经验直接反哺模型能力的提升。

对于 AI 研究团队来说，这是 Hermes Agent 不可替代的价值。对于普通用户，这个功能可能用不上，但它代表了 Hermes Agent 的长期愿景——让 Agent 越用越聪明。

---

## 十二、OpenClaw → Hermes Agent 迁移

Hermes Agent 提供了从 OpenClaw 的一键迁移工具：
```bash
hermes claw migrate           # 交互式迁移（完整预设）
hermes claw migrate --dry-run # 预览迁移内容
hermes claw migrate --preset user-data  # 不迁移密钥
hermes claw migrate --overwrite         # 覆盖已有冲突
```

迁移内容包括：SOUL.md、记忆文件、技能、命令白名单、消息平台配置、API 密钥、TTS 资源、工作区指令（AGENTS.md）。迁移工具的存在说明两个项目有一定程度的用户重叠和生态互补。

---

## 十三、平台支持

| 平台 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| macOS | ✅ 原生（含菜单栏应用） | ✅ 原生 |
| Linux | ✅ 原生 | ✅ 原生 |
| Windows | ✅ 原生 | ⚠️ 仅 WSL2 |
| Android | ✅ Node 配对 | ✅ Termux |
| iOS | ✅ Node 配对 | ❌ |

**Windows 原生支持是 OpenClaw 的显著优势。** Hermes Agent 不支持原生 Windows，必须通过 WSL2 运行，这对 Windows 用户是不小的门槛。OpenClaw 提供了 PowerShell 安装脚本，开箱即用。

---

## 十四、部署与运维

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **安装** | `npm install -g openclaw` / 安装脚本 | `pip install hermes-agent` / 安装脚本 |
| **技术栈** | Node.js | Python |
| **配置** | JSON5 + 环境变量 | YAML + .env |
| **守护进程** | Gateway（LaunchAgent/systemd/计划任务） | Gateway / systemd |
| **CLI 命令** | 50+ 子命令 | hermes + 子命令 |
| **健康检查** | `openclaw doctor` + `openclaw status --deep` | `hermes doctor` |
| **安全审计** | `openclaw security audit --fix` | — |
| **自动更新** | `openclaw update` | `hermes update` |
| **备份** | `openclaw backup create` | — |
| **Profile 管理** | — | `hermes profile`（创建/克隆/导出/导入） |
| **macOS 应用** | ✅ 菜单栏应用 | ❌ |
| **移动节点** | ✅ iOS / Android | ✅ Android (Termux) |
| **一键迁移** | — | ✅ `hermes claw migrate`（从 OpenClaw 迁入） |

OpenClaw 在**运维工具链**上明显更完善：doctor、security audit、backup、update 等工具覆盖了完整的运维生命周期。Hermes Agent 的 Profile 系统更灵活——可以创建完全隔离的多个 Agent 实例，各自有独立的配置、记忆、技能和会话。

---

## 十五、适用场景

### 选择 OpenClaw

- **Windows 用户**——唯一支持原生 Windows 的选择
- 需要精细的命令执行审批和合规审计
- 需要统一管理 10+ 消息通道
- 重视运维工具链（健康检查、安全审计、备份、自动更新）
- 团队使用国内 AI 模型（智谱 GLM、火山引擎等）
- 需要 iOS 移动节点（摄像头、定位）
- 偏好"开箱即用"的完整解决方案
- 需要视频生成能力（`video_generate`）

### 选择 Hermes Agent

- **AI 研究团队**——RL 训练闭环是核心需求
- 需要 Discord 语音频道实时交互
- 需要专业级记忆后端（Mem0、Honcho 等）
- 已有 Python 技术栈，不想引入 Node.js
- 需要 Home Assistant 智能家居集成
- 原生使用钉钉/飞书/企业微信/微信
- 需要 Agent 自主学习和积累经验（程序性记忆）
- 重视上下文管理的精细控制（独立压缩模型、prompt caching）
- 需要多终端后端隔离（Docker/SSH/Modal/Daytona）
- 需要 Webhook 接收外部 HTTP 回调触发 Agent

---

## 十六、总结

OpenClaw 和 Hermes Agent 代表了 AI Agent 框架的两种设计哲学：

**OpenClaw** 是**工程师的框架**。它追求的是可靠性、可控性和运维友好。严格的 Schema 校验、精细的审批系统、完善的 CLI 工具链、原生 Windows 支持——每一处设计都透着"生产环境"的气息。它是一个你部署后就不太需要操心的系统。

**Hermes Agent** 是**研究者的框架**。它追求的是灵活性、可扩展性和自学习能力。RL 训练闭环、程序性记忆、7 层安全纵深、5 种浏览器后端、4 种 STT/TTS 提供商——每一处设计都透着"探索边界"的气息。它是一个你越用越强的系统。

两者没有绝对的优劣，只有场景的匹配。如果你需要的是一个**稳定可靠、好运维的 AI 助手网关**，OpenClaw 是更好的选择。如果你需要的是一个**能自我进化、深度集成的 AI 研究平台**，Hermes Agent 值得深入探索。

最好的选择，可能取决于你的身份：你是工程师，还是研究者？

---

*本文基于 OpenClaw（2026.4.11）和 Hermes Agent 官方文档（2026 年 4 月版本）编写。如有不准确之处，欢迎指正。*
