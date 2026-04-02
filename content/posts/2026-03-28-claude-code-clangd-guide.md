---
title: "Claude Code + clangd 实战笔记：大代码项目的高效 AI 辅助开发"
date: 2026-03-28
tags: ["Claude Code", "clangd", "LSP", "C++", "AI编程"]
categories: ["开发工具"]
---

## 一、问题：代码量大，AI 上下文装不下

当项目代码量很大时，Claude Code 直接从头到尾阅读代码很容易超过上下文窗口限制。解决方案是让 Claude Code 通过 **LSP（Language Server Protocol）** 精准定位代码，而不是全文阅读。

### 有无 LSP 的区别

| 无 LSP | 有 LSP（clangd） |
|--------|----------------|
| 以文本形式解析代码，用 Grep 模糊搜索 | 直接调用语言服务器，获取精确语法结构 |
| 查找引用/定义需反复搜索，Token 消耗高 | 单次查询到位，Token 消耗降低 40%+ |
| 不理解类型信息 | 能获取类型签名、跨文件引用 |

没有 LSP，Claude Code 是"聪明的文本搜索"；有了 LSP，才具备 IDE 级别的代码理解能力。

---

## 二、clangd 是什么

clangd 是 LLVM/Clang 项目提供的 C/C++ 语言服务器，它能：
- 精确的代码补全（比 VS 原生 IntelliSense 在复杂模板代码中更准）
- 跨文件的符号查找（定义、引用、调用关系）
- 实时诊断（类型错误、未使用变量等）
- 代码导航（跳转到定义、查找所有引用）

### clangd 的局限

clangd **不是实时分析的**，它依赖一个 `compile_commands.json` 文件来了解每个文件的编译参数（包含路径、宏定义、编译选项等）。没有这个文件，clangd 就无法正确工作。

---

## 三、轻量方案：手动创建 .clangd 配置文件

如果你的项目结构比较简单，或者不想安装额外工具生成 `compile_commands.json`，可以手动创建一个 `.clangd` 配置文件来告诉 clangd 需要检索的包含目录。

在项目根目录创建 `.clangd` 文件（YAML 格式）：

```yaml
CompileFlags:
  Add:
    - "-IC:/path/to/include/dir1"
    - "-IC:/path/to/include/dir2"
    - "-D_SOME_MACRO"
```

### 适用场景

- 项目结构简单，不需要完整的编译参数
- 只需要让 clangd 知道几个额外的 include 路径和宏定义
- 不想安装 Clang Power Tools

### 局限性

- 没有完整的编译参数，分析精度不如 `compile_commands.json`
- 项目复杂时（多配置、条件编译多）建议使用完整的 compile_commands.json 方案

---

## 四、完整方案：从 VS .sln/.vcxproj 生成 compile_commands.json

### 安装 Clang Power Tools

1. 打开 Visual Studio
2. **扩展 → 管理扩展 → 在线**
3. 搜索 **"Clang Power Tools"**
4. 下载并安装
5. 重启 VS

### 生成 compile_commands.json

1. 打开你的 .sln 解决方案
2. **右键项目** → 选择 **"Export Compilation Database"**
3. 自动在项目目录生成 `compile_commands.json`

就这么简单，不需要 CMake，不需要改项目结构。

### 保持最新

- **日常改代码**（修改 .cpp/.h 内容）：**不需要**重新生成。编译参数没变，clangd 会自动重新分析修改的文件
- **修改项目配置**（添加/删除文件、改编译选项、改包含目录等）：需要重新右键 → Export Compilation Database

---

## 五、在 Claude Code 中确认 LSP 生效

### 步骤 1：确认 clangd 可用

在终端运行：
```powershell
where clangd
```
确认能找到路径（如 `C:\Program Files\LLVM\bin\clangd.exe`）。

### 步骤 2：查看插件状态

在 Claude Code 中输入：
```
/plugins
```
检查 `clangd-lsp` 是否显示 `✓ enabled`，Errors 标签页是否有 "Executable not found" 错误。

### 步骤 3：实际测试

在一个 C++ 项目中让 Claude Code 执行：
```
Find references to [某函数名] using LSP
```
或者问："这个函数的类型签名是什么？"

如果 LSP 正常工作，Claude 会调用语言服务器返回精确结果，而不是用 grep 搜索。

---

## 六、Claude Code + clangd 工作流程

```
1. VS 中打开 .sln
2. 右键项目 → Export Compilation Database（首次或改项目配置后）
3. 在项目根目录启动 Claude Code
4. Claude Code 的 clangd-lsp 插件自动识别 compile_commands.json
5. 让 Claude Code 通过 LSP 精准查询符号
```

即使代码量再大，Claude 也能精准定位到具体文件和函数，不会浪费上下文窗口。

---

## 七、相关工具速查

| 工具 | 用途 | 安装方式 |
|------|------|---------|
| LLVM/Clang | 提供 clangd 可执行文件 | llvm.org 下载安装，添加到 PATH |
| Clang Power Tools | 从 .sln 生成 compile_commands.json | VS 扩展商店安装 |
| Claude Code | AI 编码助手 | claude.ai 下载，终端使用 |
| clangd-lsp 插件 | Claude Code 连接 clangd 的桥梁 | Claude Code 中 /plugin 安装 |

---

## 八、注意事项

- clangd 对 MFC 项目的支持不如标准 C++ 完善，可能对一些 MFC 特有的宏和类型推断不准
- compile_commands.json 可以提交到版本控制，也可以加到 .gitignore（看团队习惯）
- 如果 LSP 出问题导致 Claude Code 工作异常，可以在 /plugins 中禁用 clangd-lsp，Claude Code 会回退到 grep 模式
