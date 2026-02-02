# gf-ai-context

Lightweight AI instructions for GoFrame (GF) framework development.

## Overview

`gf-ai-context` provides concise, high-priority instructions for AI coding assistants. It is designed to be loaded into the AI's context window and guide task routing and code generation.

## Structure

```
gf-ai-context/
├── 00-instructions.md   # Core rules, file priority, decision tree
└── workflows.md         # Step-by-step workflows for common tasks
```

## Key Features

- **CLI-First Approach**: Prioritize `gf-cli` tools over manual coding
- **Three-Tier Architecture**: API → Controller → Logic → DAO
- **Quick Reference Table**: Direct links to gf-skills detailed documentation
- **Decision Tree**: Quick routing for common development tasks

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
| FAQ | Common issues and solutions |

## Usage

Include the contents of `00-instructions.md` in your AI assistant's system prompt or reference it as a rules file.

## Related Projects

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - Comprehensive knowledge base (9 reference files)
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP execution tools

## License

MIT
