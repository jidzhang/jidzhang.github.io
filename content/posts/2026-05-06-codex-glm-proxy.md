---
title: "用 GLM 模型驱动 OpenAI Codex CLI：codex-glm-proxy 使用指南"
date: 2026-05-06
tags: ["GLM", "Codex CLI", "智谱AI", "AI编程", "开源"]
categories: ["技术"]
---

OpenAI Codex CLI 是一个强大的终端 AI 编程助手，但它默认只支持 OpenAI 自家的模型。如果你更倾向于使用国内的大模型，比如智谱 AI 的 GLM 系列，该怎么办？

**codex-glm-proxy** 就是解决这个问题的工具——一个轻量级的本地代理，让 Codex CLI 无缝对接 GLM 模型。

## 它做了什么？

简单来说，Codex CLI 使用的是 OpenAI 的 **Responses API** 格式，而智谱 GLM 使用的是 **Chat Completions API** 格式。两者协议不同，无法直接通信。

codex-glm-proxy 在中间做双向格式转换：

```
Codex CLI ──Responses API──▶ 本地代理 (localhost:18765) ──Chat Completions──▶ GLM API
```

你不需要改 Codex 的源码，也不需要等官方适配，启动代理、改个配置文件就行。

## 核心特性

- **流式响应**：实时 SSE 流式输出，打字效果和 OpenAI 原生体验一致
- **工具调用**：完整支持 `apply_patch`、`exec` 等 Codex 全部工具，AI 可以直接帮你改代码、跑命令
- **多轮对话**：完整保持上下文，支持连续追问和迭代
- **连接池复用**：内置 TCP+TLS 连接池，多实例并发时性能更好
- **单文件无依赖**：整个代理就一个 `proxy.py`，纯 Python 标准库，不用 pip install 任何东西

## 支持哪些模型？

代理支持智谱 GLM 全系列 Coding 模型：

- **GLM-5.1**：最强推理能力，适合复杂编码和架构设计
- **GLM-5-Turbo**：速度更快，适合日常编码
- **GLM-5**：标准模型
- **GLM-4.7**：轻量高效，适合简单任务

如果 Codex 请求的是 OpenAI 模型名（如 `gpt-4o`），代理会自动映射到对应的 GLM 模型，无需手动指定。

## 5 分钟上手

### 前置条件

- Python 3.8+
- 智谱 AI API Key（[点此获取](https://www.bigmodel.cn/glm-coding?ic=PUT51M7I5Q)）
- 已安装 [OpenAI Codex CLI](https://github.com/openai/codex)

### 第一步：启动代理

```bash
git clone https://gitee.com/jidzhang/codex-glm-proxy.git
cd codex-glm-proxy

export GLM_API_KEY="你的API密钥"
python3 proxy.py
```

代理默认监听 `http://localhost:18765`。也可以用后台脚本：

```bash
./scripts/start.sh    # 后台启动
./scripts/stop.sh     # 停止
```

### 第二步：配置 Codex CLI

创建或编辑 `~/.codex/config.toml`：

```toml
model_provider = "glm"
model = "glm-5.1"

model_catalog_json = "/你的用户目录/.codex/models.json"

[model_providers.glm]
name = "GLM via Proxy"
base_url = "http://localhost:18765"
wire_api = "responses"
```

### 第三步：设置模型目录

把项目里的 `models.json` 复制到 `~/.codex/models.json`：

```bash
cp models.json ~/.codex/models.json
```

### 第四步：开用

```bash
mkdir my-project && cd my-project && git init
codex "用 Python 写一个猜数字游戏"
```

Codex 会通过代理调用 GLM 模型，自动生成代码并执行。

## 推理深度说明

Codex CLI 支持设置推理深度（reasoning effort），控制模型回答前的思考程度。经过实际测试：

- **low**：快速响应，简单任务够用
- **medium**：平衡模式
- **high**：深度推理，效果最好，**推荐使用**

注意：`xhigh` 在 GLM API 中并不比 `high` 更好，会被静默忽略。提供的 `models.json` 已默认设为 `high`，开箱即用。

## 管理和排障

```bash
curl http://localhost:18765/health     # 健康检查
tail -f /tmp/codex-glm-proxy.log       # 查看日志
```

常见问题：

- **"Streaming complete, sent 0 chunks"**：模型名没配对，确保 config.toml 中 model 写的是 `glm-5.1` 等
- **Connection refused**：代理没启动，先跑 `python3 proxy.py`
- **Codex 循环重复操作**：更新到最新版代理

## 关于 GLM Coding Plan

如果你想使用智谱 GLM 的 Coding 功能，推荐了解一下 **GLM Coding Plan**：

👉 [https://www.bigmodel.cn/glm-coding?ic=PUT51M7I5Q](https://www.bigmodel.cn/glm-coding?ic=PUT51M7I5Q)

这是智谱官方推出的编程专项方案，提供适合编码场景的模型能力和 API 资源。

## 项目地址

- **Gitee**：[https://gitee.com/jidzhang/codex-glm-proxy](https://gitee.com/jidzhang/codex-glm-proxy)
- **GitHub 原项目**：[https://github.com/JichinX/codex-glm-proxy](https://github.com/JichinX/codex-glm-proxy)

MIT 协议，欢迎 Star、Fork 和 PR。

---

**总结**：codex-glm-proxy 用最简单的方式打通了 Codex CLI 和 GLM 模型。单文件、零依赖、配置即用。如果你想在终端里用 GLM 写代码，值得一试。
