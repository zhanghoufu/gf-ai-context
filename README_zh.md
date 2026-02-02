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

将 `00-instructions.md`（以及按需的 `workflows.md`、`tools.md`、`patterns.md`）纳入 AI 助手的系统提示或作为规则文件引用。

## 相关项目

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - 综合知识库（references + best-practices + troubleshooting）
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP 执行工具（gf_init、gf_gen_dao、gf_gen_service、gf_gen_ctrl、gf_run 等）

## 许可证

MIT
