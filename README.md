# gf-ai-context

[中文](README_zh.md) | English

Lightweight AI instructions for GoFrame (GF) framework development. 

## Overview

`gf-ai-context` provides concise, high-priority instructions for AI coding assistants. It is designed to be loaded into the AI's context window and guide task routing and code generation.

## Three-Layer AI Assistance

```
┌─────────────────────────────────────────────────────────────┐
│                     AI Assistant                             │
│  (Claude Code, Cursor, Copilot, Windsurf)                    │
└────────────┬─────────────────────┬──────────────────────────┘
             │                     │
             ├─ Workflow Layer ────┤
             │  gf-ai-context      │  "What to do" - Decision tree,
             │  (~15KB)            │  workflows, tools, patterns
             │                     │
             ├─ Knowledge Layer ───┤
             │  gf-skills         │  "How & Why" - Detailed patterns
             │  (~40KB+)          │  Load when needed
             │                     │
             └─ Execution Layer ───┘
                gf-mcp / gf-cli     "Do it" - Code generation
                (MCP or CLI)        Actual file generation
```

| Component | Purpose | Repository |
|-----------|---------|------------|
| **gf-ai-context** | Workflow, tools, quick patterns | This repo |
| **gf-skills** | Detailed patterns, best practices | [gf-skills](https://github.com/zhanghoufu/gf-skills) |
| **gf-mcp** | MCP tools (gf_init, gf_gen_*, gf_run) | [gf-mcp](https://github.com/zhanghoufu/gf-mcp) |

## Structure

| File | Purpose |
|------|---------|
| `00-instructions.md` | Core rules, file priority, decision tree, gf-skills links |
| `workflows.md` | Step-by-step workflows + decision matrix |
| `tools.md` | MCP tools and gf-cli commands, tool sequences |
| `patterns.md` | Quick code patterns (API, Controller, Logic, Config, DAO) |

## Key Features

- **CLI-First / MCP-First**: Prefer gf-mcp tools or gf-cli over manual coding
- **Three-Tier Architecture**: API → Controller → Logic → DAO
- **Decision Matrix**: Request type → workflow mapping
- **Quick Patterns**: patterns.md for common snippets; deep patterns in gf-skills

## gf-skills Reference

| Topic | When to Use |
|-------|-------------|
| Core Components | gcfg, glog, gerror, gvalid, gconv, gcache, gcmd, gview, gi18n, gres |
| Components List | Full component reference |
| Web Development | HTTP server, routing, middleware, OpenAPI |
| Database & ORM | DAO, transactions, queries |
| CLI Automation | gf-cli commands |
| Microservices | gRPC, service discovery |
| Deployment | Standalone, Docker, Kubernetes |
| FAQ / Troubleshooting | Common issues and solutions |

## Usage

Include the contents of `00-instructions.md` (and optionally `workflows.md`, `tools.md`, `patterns.md`) in your AI assistant's system prompt or reference them as rules files.

## Related Projects

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - Comprehensive knowledge base (references + best-practices + troubleshooting)
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP execution tools (gf_init, gf_gen_dao, gf_gen_service, gf_gen_ctrl, gf_run, etc.)

## License

MIT
