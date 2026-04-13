---
title: "OpenClaw 2026.3.31 升级后疯狂弹审批？改这两个配置就够了"
date: 2026-04-04
tags: ["OpenClaw", "配置", "排障"]
categories: ["技术"]
---

## 问题

升级 OpenClaw 2026.3.31 后，执行命令频繁弹出 `/approve` 要求手动审批。关掉 `ask` 之后命令还是报 `exec denied: allowlist miss`，依然跑不了。

## 原因

OpenClaw 2026.3.31 把执行权限拆成了**两层**：

1. **审批层**（`ask`）—— 决定要不要弹窗让你批准
2. **执行层**（`security`）—— 决定命令有没有资格执行

很多人只改了第一层，第二层没动，所以审批没了但命令还是死。**不弹窗 ≠ 放行。**

## 解决方案

打开 `~/.openclaw/openclaw.json`，确保 `tools.exec` 同时配置这两项：
```json
{
  "tools": {
    "exec": {
      "ask": "off",
      "security": "full"
    }
  }
}
```

- `ask: "off"` —— 不再弹审批
- `security: "full"` —— 放开执行权限

改完后执行：
```bash
openclaw config validate
openclaw gateway restart
```

用一个最轻的命令验证：
```bash
pwd
```

能直接返回路径就说明生效了。

## 注意事项

这套配置适合**单人使用、追求效率**的场景。如果你的 OpenClaw 是多人共享或挂在群聊里，`security: "full"` 风险较大，建议用 `allowlist` 模式精细控制。
