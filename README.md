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

## One-Prompt Setup

Tell your AI assistant:

```
Set up GoFrame AI tools for this project from https://github.com/zhanghoufu/gf-ai-context
```

The AI can then:
1. Detect your AI tool (Claude Code, Cursor, Copilot, Windsurf)
2. Install the matching configuration
3. Wire in gf-skills for detailed patterns
4. Configure gf-mcp (if using an environment that supports MCP)

## Manual Setup

If you prefer to install by hand, pick your AI tool below.

### Claude Code (Recommended)

```bash
# Install gf-ai-context (workflow instructions)
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .claude/gf-ai-context

# Install gf-skills (knowledge base)
git submodule add https://github.com/zhanghoufu/gf-skills.git .claude/skills/gf-skills

# Install gf-mcp (code generation tools) — optional, in your home dir
git clone https://github.com/zhanghoufu/gf-mcp.git ~/.gf-mcp
cd ~/.gf-mcp && go build -o gf-mcp .

# Register gf-mcp in Claude Code (optional)
claude mcp add gf-mcp --transport stdio -- ~/.gf-mcp/gf-mcp
```

Ensure `gf` (GoFrame CLI) is on your `PATH` so gf-mcp can run it.

### Cursor

```bash
# Add gf-ai-context as rules directory (Cursor reads all .md files in it)
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .cursorrules

# Optional: add gf-skills for reference
git submodule add https://github.com/zhanghoufu/gf-skills.git .cursorrules/gf-skills
```

### GitHub Copilot

```bash
# Add gf-ai-context
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .github/gf-ai-context

# Point Copilot at the main instructions
ln -s gf-ai-context/00-instructions.md .github/copilot-instructions.md

# Optional: add gf-skills for reference
git submodule add https://github.com/zhanghoufu/gf-skills.git .github/gf-skills
```

### Windsurf

```bash
# Add gf-ai-context as rules directory
git submodule add https://github.com/zhanghoufu/gf-ai-context.git .windsurfrules

# Optional: add gf-skills for reference
git submodule add https://github.com/zhanghoufu/gf-skills.git .windsurfrules/gf-skills
```

## After Setup

Once installed, your AI assistant can:

**Generate services:**
```
Create a user management API with CRUD and gf gen dao/ctrl/service
```

**Apply patterns:**
```
Add request validation with v tags and gerror in my logic layer
```

**Troubleshoot:**
```
Why does gf gen service not see my new logic method?
```

**Review practices:**
```
Check my controller for GoFrame anti-patterns (business logic in controller)
```

## Updating

Keep gf-ai-context and gf-skills up to date:

```bash
# Update all submodules
git submodule update --remote --recursive

# Or update by path
git submodule update --remote .claude/gf-ai-context
git submodule update --remote .claude/skills/gf-skills
git submodule update --remote .cursorrules
git submodule update --remote .github/gf-ai-context
git submodule update --remote .windsurfrules
```

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

If not using Manual Setup above: include `00-instructions.md` (and optionally `workflows.md`, `tools.md`, `patterns.md`) in your AI assistant’s system prompt or point your tool’s rules at this directory.

## Related Projects

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - Comprehensive knowledge base (references + best-practices + troubleshooting)
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP execution tools (gf_init, gf_gen_dao, gf_gen_service, gf_gen_ctrl, gf_run, etc.)

## License

MIT
