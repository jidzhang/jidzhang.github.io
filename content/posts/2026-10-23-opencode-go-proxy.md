---
title: "opencode-go-proxy：让 Codex 和 Claude Code 用上 OpenCode Go 的 38 个模型"
date: 2026-10-23
tags: ["OpenCode Go", "Codex CLI", "Claude Code", "AI 编程", "本地代理"]
categories: ["项目"]
---

我平时 Codex 和 Claude Code 都用。两边的模型池不一样，想换 DeepSeek、GLM 或者 GPT-6 Luna 的时候，经常得为了一个模型换客户端，挺烦的。OpenCode Go 把这些模型放进了一个 10 美元/月的订阅里，但订阅只解决"用哪些模型"，没解决"在哪儿用"：

- Codex 说的是 Responses API；
- Claude Code 说的是 Anthropic Messages；
- 而 OpenCode Go 的模型来自不同厂商，上游横跨 Responses、Chat Completions、Anthropic Messages、Google `generateContent` 四种协议。

**opencode-go-proxy** 就是中间那层本地适配器：一个进程、一个端口，把客户端的请求翻译成模型认识的上游协议，再把流式响应翻译回来。

```text
Codex app ── POST /v1/responses (Responses API) ──┐
                                                  │
Claude Code ── POST /v1/messages (Anthropic) ─────┤
                                                  ▼
                                opencode-go-proxy  ←── 127.0.0.1:8787
                                                  │
     POST per family: chat/completions · responses · messages · models/<id>:generateContent
                                                  ▼
                         OpenCode Go / OpenCode Zen  ── 38 个认证模型 · 可选 Zen 模型
```

## 问题不在模型，在协议

把模型接进客户端，最直接的做法是写个转发脚本。真用起来才发现，转发本身倒不难，麻烦的是各家协议在细节上对不上：

| 上游协议 | 代表模型 |
|---|---|
| Responses `/responses` | `gpt-6-luna`、`gpt-5.6-luna`、`muse-spark-1.3-contributor` |
| Chat Completions `/chat/completions` | `deepseek-v4.1-flash`、`glm-5.3-flash`、`kimi-k3`、`mimo-v2.6-flash` |
| Anthropic Messages `/messages` | `claude-haiku-5-5`、`minimax-m3`、`qwen3.8-max` |
| Google `:generateContent` | `gemini-4-argon-contributor` |

鉴权头不一样，流式事件不一样，工具调用的 JSON 结构也不一样，报错格式还各写各的。所以光做字段映射不够，得让客户端基本感觉不到中间多了一层。

## 它到底做了什么

对客户端，它同时提供三个入口：

| 客户端 | 入口 | 能用的模型 |
|---|---|---|
| Codex | `POST /v1/responses` | 38 个 Go 认证模型 + Zen 模型 + 你账号里的原生 GPT 模型 |
| Claude Code | `POST /v1/messages` | 38 个 Go 认证模型（9 个 Messages 模型直通，其余 29 个翻译） |
| 任意 OpenAI SDK | `POST /v1/chat/completions` | 23 个 Chat Completions 模型 |

路由按模型名决定：`opencode-go/<id>` 明确走 Go，`zen/<id>` 走 Zen 按量计费，裸的原生模型 ID 继续走 ChatGPT 后端。**未知或写错的模型会在请求上游之前被拒绝，不会静默换成另一个模型**——这一点在排查问题时很重要。

## 光会转发还不够

协议翻译只能算"能跑"。真正天天用起来，下面这些事更影响体验：

- **空流重试**：上游偶尔返回 200，但 SSE 里没有内容。Agent 会以为任务已完成，然后停在半路。代理会重试；要是最后一次还是空，它会直接报错，不会给你一个看起来正常、其实啥也没有的响应。
- **工具调用与 reasoning 回放**：Codex 的 `apply_patch`、自定义 freeform 工具、跨轮 `tool_use` ID、reasoning content，都要原样保留，否则多轮任务会断。
- **前缀缓存**：系统提示、工具定义、历史消息保持字节稳定，并让上游回传 usage。命中率可以在 `/cache` 里看，长会话能省下大量输入 token。
- **图片降级**：请求的模型不支持图片时，自动把图片转成文字描述再重试，不至于整轮白跑。
- **配额终态**：Go 额度耗尽时归一化成 `insufficient_quota`，Codex 会立刻停下，而不是干等一个几小时后才重置的窗口。
- **可观测**：`/state`、`/quota`、`/cache` 加上分级 JSON 日志，能回答"钱花哪了、缓存命中多少、还剩多少额度"。
- **授权隔离**：原生 ChatGPT/Codex 登录态不动；远程客户端的 caller token 只用于访问代理，永远不会被转发到上游。

这些事单拎出来都不算大，但少了哪个，都可能在某天突然卡你一下。

## 快速开始

### 0. 准备

- Python 3.11+，以及 [uv](https://docs.astral.sh/uv/)
- OpenCode Go 订阅的 API Key（在 [OpenCode Zen 后台](https://opencode.ai/zen) 复制）
- 已安装 Codex CLI 或 Claude Code

API Key 通过环境变量提供：

```bash
export OPENCODE_GO_API_KEY="你的 key"
```

Windows PowerShell 用：

```powershell
$env:OPENCODE_GO_API_KEY = "你的 key"
```

### 1. 启动代理

```bash
uvx --from git+https://github.com/jidzhang/opencode-go-proxy \
  opencode-go-proxy --bind 127.0.0.1 --port 8787
```

默认只监听本机回环地址。看到 `server.start` 之类的日志就说明起来了。

### 2. 配置 Codex

```bash
uvx --from git+https://github.com/jidzhang/opencode-go-proxy \
  opencode-go-proxy config enable
```

它只动 `~/.codex/config.toml` 里自己标记的那一块，写 `openai_base_url` 和 `model_catalog_json`。你原来的配置它不碰；如果这两个键你自己已经设过，它会拒绝覆盖。想撤销就 `config disable`，也只删自己写的那块。

然后**完全退出并重启 Codex**，选择模型：

```bash
codex -m opencode-go/gpt-6-luna
```

第一次启动时，代理会在状态目录（默认 `~/.codex/opencode-go-proxy/`）生成模型目录。如果模型选择器里看不到，先确认代理已经跑起来，再重启 Codex。

### 3. 配置 Claude Code

Claude Code 不需要改配置文件，三个环境变量就够：

```bash
export ANTHROPIC_BASE_URL="http://127.0.0.1:8787"
export ANTHROPIC_AUTH_TOKEN="local"
export ANTHROPIC_MODEL="opencode-go/claude-haiku-5-5"
claude
```

本机访问不看 token，填个 `local` 就行。只有从别的机器连过来，才需要填真正的 caller token。

PowerShell 下把 `export` 换成 `$env:NAME = "..."`，效果一样。

### 4. 验证

```bash
curl http://127.0.0.1:8787/health
curl http://127.0.0.1:8787/v1/models | jq '.data[].id' | head
```

`/v1/models` 里应该同时看到 `opencode-go/...`、`zen/...`，以及你账号里的原生模型。

## 日常怎么用

**切模型**就是换一个前缀：

```bash
codex -m opencode-go/gpt-6-luna           # Responses 直连
codex -m opencode-go/deepseek-v4.1-flash  # Chat Completions
codex -m opencode-go/glm-5.3-flash        # Chat Completions
codex -m opencode-go/claude-haiku-5-5     # Anthropic Messages
```

Claude Code 用 `claude --model opencode-go/claude-haiku-5-5`，或者改 `ANTHROPIC_MODEL`。

**看状态**：

```bash
curl http://127.0.0.1:8787/state   # 状态、今日用量、7 天趋势、当前模型
curl http://127.0.0.1:8787/quota   # 上游返回的配额快照
curl http://127.0.0.1:8787/cache   # 每个模型的前缀缓存命中率
```

默认日志级别是 `info`；想看逐请求细节时用 `OPENCODE_GO_PROXY_LOG_LEVEL=debug`，稳定之后调到 `warn`，健康的代理就不会每轮都刷日志。

**用 Zen 模型**：`zen/<id>` 走按量计费，不占 Go 配额；`deepseek-v4-flash-free`、`mimo-v2.5-free`、`big-pickle` 这几个免费模型不需要余额。Zen 目录是从 `opencode.ai/zen/v1/models` 自动发现的，不用手工维护。

**原生 GPT 模型共存**：裸模型 ID（比如你账号里的官方模型）仍然走 ChatGPT 后端，用的是 Codex 自己的登录态。Go 模型和官方模型会一起出现在模型选择器里，互不影响。

**图片和工具**：正常发就行。带工具的图片轮次会自动走 caption 路径；模型不支持图片时也会自动降级并重试一次。

## 放到局域网或服务器

默认只监听 `127.0.0.1`，这是最安全的用法。如果要从另一台设备访问，必须同时打开远程模式并设置 caller token：

```bash
export OPENCODE_GO_PROXY_ALLOW_REMOTE=1
export OPENCODE_GO_PROXY_CALLER_TOKEN="$(openssl rand -hex 32)"
opencode-go-proxy --bind 0.0.0.0 --port 8787
```

Windows 上没有 `openssl` 的话，用任意 32 位以上的随机字符串代替即可。

三种客户端都能带上这个 token：

| 客户端 | 传递方式 | 配置位置 |
|---|---|---|
| Codex | `X-OpenCode-Go-Proxy-Token` 请求头 | 自定义 provider 的 `env_http_headers` |
| zcode | `Authorization: Bearer <token>` | provider 的 `apiKey` |
| Claude Code | `x-api-key: <token>` | `ANTHROPIC_AUTH_TOKEN` |

Codex 的 provider 可以这样写，token 从环境变量读取，不落盘：

```toml
[model_providers.opencode-go-remote]
name = "OpenCode Go Remote"
base_url = "https://proxy.example.com/v1"
wire_api = "responses"
env_http_headers = { "X-OpenCode-Go-Proxy-Token" = "OPENCODE_GO_PROXY_CALLER_TOKEN" }
```

几点提醒：

- 真要放到公网，前面记得套 TLS，最好再加一层网络访问控制；
- caller token 只是进代理的门票，不是模型 API Key，也不会被转发给上游；
- 这套东西是给**自己的几台设备**用的，不是多租户网关。账号共享、转售这些事别干，项目本身也不支持。

## 开机自启

- **Windows**：仓库里带了 NSSM 脚本（`install-service.bat`、`restart-service.bat`、`stop-service.bat`），装好 NSSM 并设好前面的环境变量后，以管理员身份运行即可注册成服务。脚本里的端口、绑定地址和日志路径按自己的环境改。
- **macOS**：`macos/MenuBarApp` 是菜单栏应用，会自己拉起代理并显示状态、配额和今日用量。
- **Linux**：`contrib/systemd/opencode-go-proxy.service` 是现成的 systemd user unit。

同一个端口只能有一个实例。启动前可以先确认端口有没有被占用，避免起了两个代理互相打架。

## 边界和注意事项

- **单用户、单 key**。一份订阅对应一个 OpenCode 工作区，代理不做账号池，也不帮你绕过限额。
- **配额是美元制**：滚动 5 小时 12 美元、每周 30 美元、每月 60 美元。贵模型烧得快，日常可以多用 DeepSeek V4.1 Flash 或 MiMo 这类便宜模型。
- **上游会变**：模型列表、协议归属、价格都可能调整。代理只接受文档化或经过协议验证的 ID，未知 ID 直接拒绝，不会猜一个协议硬发。
- **部分模型有区域限制或数据政策差异**，比如 Muse Spark Contributor 系列可能用请求数据做训练，不是零留存。用之前看一遍 OpenCode 的当前文档。
- 项目还在快速迭代，写作时是 0.4.10，基本一个改动发一个小版本，更新挺勤的。

## 项目地址

- 本仓库：[github.com/jidzhang/opencode-go-proxy](https://github.com/jidzhang/opencode-go-proxy)
- 上游项目：[github.com/kartikkabadi/opencode-go-proxy](https://github.com/kartikkabadi/opencode-go-proxy)
- License：MIT

这个仓库是上游项目的分支。基础架构和大部分功能来自 Kartik Kabadi；我在日常使用中陆续补了 Claude Code 全模型支持、Gemini 原生协议、空流重试、分级日志和 Windows 服务脚本，也把踩过的坑写进了测试里。

配置项、模型清单、排障命令这些，README 里写得更全。

---

如果你也有 OpenCode Go 订阅，平时主要在 Codex 或 Claude Code 里写代码，可以试试。它不会让模型变强，但能让你少折腾协议、重试、计量和配额这些事——都收在一个本地进程里了。
