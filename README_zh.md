# gf-ai-context

中文 | [English](README.md)

GoFrame (GF) 框架开发的轻量级 AI 指令
## 概述

`gf-ai-context` 为 AI 编码助手提供简洁的高优先级指令，被设计为加载到 AI 的上下文窗口中，指导任务路由和代码生成。

## 三层 AI 辅助结构

```
┌─────────────────────────────────────────────────────────────┐
│                     AI 助手                                  │
│  (Claude Code, Cursor, Copilot, Windsurf)                   │
└────────────┬─────────────────────┬──────────────────────────┘
             │                     │
             ├─ 工作流层 ──────────┤
             │  gf-ai-context      │  “做什么” - 决策树、工作流、
             │  (~15KB)            │  工具与快速代码模式
             │                     │
             ├─ 知识层 ────────────┤
             │  gf-skills         │  “如何与为什么” - 详细模式
             │  (~40KB+)          │  需要时加载
             │                     │
             └─ 执行层 ────────────┘
                gf-mcp / gf-cli     “执行” - 代码生成
                (MCP 或 CLI)        实际文件生成
```

| 组件 | 用途 | 仓库 |
|------|------|------|
| **gf-ai-context** | 工作流、工具、快速模式 | 本仓库 |
| **gf-skills** | 详细模式与最佳实践 | [gf-skills](https://github.com/zhanghoufu/gf-skills) |
| **gf-mcp** | MCP 工具（gf_init、gf_gen_*、gf_run 等） | [gf-mcp](https://github.com/zhanghoufu/gf-mcp) |

## 目录结构

| 文件 | 用途 |
|------|------|
| `00-instructions.md` | 核心规则、文件优先级、决策树、gf-skills 链接 |
| `workflows.md` | 分步工作流 + 决策矩阵 |
| `tools.md` | MCP 工具与 gf-cli 命令、工具序列 |
| `patterns.md` | 快速代码模式（API、Controller、Logic、Config、DAO） |

## 一键说明

告诉你的 AI 助手：

```
为当前项目从 https://github.com/zhanghoufu/gf-ai-context 配置 GoFrame AI 工具
```

AI 会据此：
1. 识别你使用的 AI 工具（Claude Code、Cursor、Copilot、Windsurf）
2. 安装对应配置
3. 接入 gf-skills 详细模式
4. 若环境支持 MCP，可配置 gf-mcp

## 手动安装（Manual Setup）

若希望自己安装，请按使用的 AI 工具选择下面一种方式。

### Claude Code（推荐）

```bash
# 安装 gf-ai-context（工作流指令）
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .claude/gf-ai-context

# 安装 gf-skills（知识库）
git submodule add https://github.com/zhanghoufu/gf-skills.git .claude/skills/gf-skills

# 安装 gf-mcp（代码生成工具，可选，放在用户目录）
git clone https://github.com/zhanghoufu/gf-mcp.git ~/.gf-mcp
cd ~/.gf-mcp && go build -o gf-mcp .

# 在 Claude Code 中注册 gf-mcp（可选）
claude mcp add gf-mcp --transport stdio -- ~/.gf-mcp/gf-mcp
```

请确保本机已安装 GoFrame CLI（`gf`）并加入 `PATH`，以便 gf-mcp 调用。

### Cursor

```bash
# 将 gf-ai-context 作为规则目录（Cursor 会读取其中所有 .md）
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .cursorrules

# 可选：添加 gf-skills 供查阅
git submodule add https://github.com/zhanghoufu/gf-skills.git .cursorrules/gf-skills
```

### GitHub Copilot

```bash
# 添加 gf-ai-context
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .github/gf-ai-context

# 让 Copilot 使用主指令文件
ln -s gf-ai-context/00-instructions.md .github/copilot-instructions.md

# 可选：添加 gf-skills 供查阅
git submodule add https://github.com/zhanghoufu/gf-skills.git .github/gf-skills
```

### Windsurf

```bash
# 将 gf-ai-context 作为规则目录
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .windsurfrules

# 可选：添加 gf-skills 供查阅
git submodule add https://github.com/zhanghoufu/gf-skills.git .windsurfrules/gf-skills
```

## 安装后可以做什么（After Setup）

安装完成后，AI 助手可以：

**生成服务：**
```
用 gf gen dao/ctrl/service 做一个用户管理 API 的 CRUD
```

**应用模式：**
```
在 logic 层用 v 标签和 gerror 做请求校验与错误处理
```

**排查问题：**
```
为什么 gf gen service 没有识别到我新写的 logic 方法？
```

**代码审查：**
```
检查我的 controller 是否有 GoFrame 反模式（业务逻辑写在 controller 里）
```

## 更新（Updating）

保持 gf-ai-context 与 gf-skills 为最新：

```bash
# 更新所有子模块
git submodule update --remote --recursive

# 或按路径单独更新
git submodule update --remote .claude/gf-ai-context
git submodule update --remote .claude/skills/gf-skills
git submodule update --remote .cursorrules
git submodule update --remote .github/gf-ai-context
git submodule update --remote .windsurfrules
```

## 主要特性

- **CLI 优先 / MCP 优先**：优先使用 gf-mcp 或 gf-cli，避免手写样板代码
- **三层架构**：API → Controller → Logic → DAO
- **决策矩阵**：请求类型 → 对应工作流
- **快速模式**：patterns.md 提供常用代码片段；深入模式在 gf-skills

## gf-skills 参考

| 主题 | 使用场景 |
|------|----------|
| 核心组件 | gcfg, glog, gerror, gvalid, gconv, gcache, gcmd, gview, gi18n, gres |
| 组件列表 | 完整组件参考 |
| Web 开发 | HTTP 服务器、路由、中间件、OpenAPI |
| 数据库 & ORM | DAO、事务、查询 |
| CLI 自动化 | gf-cli 命令 |
| 微服务 | gRPC、服务发现 |
| 部署 | 独立部署、Docker、Kubernetes |
| FAQ / 故障排查 | 常见问题与解决方案 |

## 使用方法

若未按上文「手动安装」操作：可将 `00-instructions.md`（以及按需的 `workflows.md`、`tools.md`、`patterns.md`）纳入 AI 助手的系统提示，或将所用工具的规则目录指向本仓库。

## 相关项目

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - 综合知识库（references + best-practices + troubleshooting）
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP 执行工具（gf_init、gf_gen_dao、gf_gen_service、gf_gen_ctrl、gf_run 等）

## 许可证

MIT
