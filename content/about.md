---
title: 关于
date: 2024-01-01
draft: false
menu:
  main:
    weight: 5
---

## 疯狂的键盘

爱软件爱开发。

一个常年扎根在 Windows 与 C++ 世界的开发者，对老旧工具链（VS2005/2008、C++03）的兼容性有种执念，同时对 AI 编码工具链充满热情——总想着把手边的工具打磨得好用一点。

下面是我在 GitHub 上持续维护的几个重点项目，也欢迎到我的主页逛逛：[github.com/jidzhang](https://github.com/jidzhang)。

### AI 编码工具链

**[codex-glm-proxy](https://github.com/jidzhang/codex-glm-proxy)** — 让 OpenAI Codex CLI 用上智谱 GLM

一个本地代理，把 Codex CLI 使用的 Responses API 协议实时转换成 Chat Completions 协议，从而让 Codex 能够调用智谱 AI 的 GLM 系列模型（glm-5.2 / glm-4.7 等）。单文件、零外部依赖，支持流式输出、完整的工具调用（`apply_patch`、`exec` 等）和多轮对话；内置 OpenAI → GLM 的模型名映射、reasoning effort 分档控制，还能借助 NSSM 跑成 Windows 后台服务。

**[mcp-fileencoding](https://github.com/jidzhang/mcp-fileencoding)** — 让 AI 助手正确读写 GBK/GB18030 文件

一个 MCP 服务器，专治 Windows 中文环境下 AI 编码助手读取非 UTF-8 源文件（GBK、GB18030）时的乱码问题。读取时自动检测编码并转成 UTF-8 返回给 AI，写回时再自动转成原始编码，整个过程对 AI 完全透明。配合 Claude Code 的 PreToolUse Hook 可以做到按文件类型自动触发，比写在系统提示词里更可靠。

### C++ / Windows 系统级库

**[nssm](https://github.com/jidzhang/nssm)** — 把任何程序变成 Windows 服务

NSSM（Native System Service Manager）的维护分支。它能把任意可执行程序包装成 Windows NT 服务，监控其运行、崩溃后自动重启。在 fork 原项目的基础上做了现代化维护：严格保持 C++03 与 VS2008+ 兼容，补齐了基于 catch2 的 60 个单元测试与 Pester 集成测试，并引入 RAII 风格的资源守卫（`HandleGuard`、`RegistryKeyGuard` 等）。

**[simpleini](https://github.com/jidzhang/simpleini)** — 单头文件 INI 配置库

brofield/simpleini 的 fork，一个跨平台、单头文件的 INI 读写库，`#include "SimpleIni.h"` 即可使用，无需 CMake 或额外的链接配置。这个分支的重点是**严格的工具链兼容性**：公开头文件保持 C++03 标准，从 VS2005 到最新的 GCC / Clang / MSVC 都能无警告编译；并补齐了 GoogleTest 全行为测试 + C++03 轻量测试两套测试矩阵，内置严格的 UTF-8 校验。

**[smart_ptr](https://github.com/jidzhang/smart_ptr)** — C++98/03 智能指针库

为老旧的 C++98/03 工程（VS2005+）补上现代智能指针的 `shared_ptr` / `weak_ptr` / `unique_ptr`。提供单线程（零原子开销）和多线程（原子引用计数）两个版本，线程安全语义与 `std::shared_ptr` 完全一致，并通过竞态条件专项测试和百次循环压力测试验证可靠性。

---

如果你对这些项目有兴趣，无论是使用、提 issue，还是单纯交流技术，都欢迎在 GitHub 上找我。
