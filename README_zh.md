# gf-ai-context

中文 | [English](README.md)

GoFrame (GF) 框架开发的轻量级 AI 指令。

## 概述

`gf-ai-context` 为 AI 编码助手提供简洁的高优先级指令。它被设计为加载到 AI 的上下文窗口中，指导任务路由和代码生成。

## 目录结构

```
gf-ai-context/
├── 00-instructions.md   # 核心规则，文件优先级，决策树
└── workflows.md         # 常见任务的标准化工作流
```

## 主要特性

- **CLI优先**: 优先使用 `gf-cli` 工具而非手动编码
- **三层架构**: API → Controller → Logic → DAO
- **快速参考表**: 直接链接到 gf-skills 详细文档
- **决策树**: 快速路由常见开发任务

## gf-skills 参考

| 主题 | 使用场景 |
|------|----------|
| 核心组件 | gcfg, glog, gerror, gvalid, gconv, gcache, gcmd, gview, gi18n, gres |
| 组件列表 | 完整组件参考 |
| Web开发 | HTTP服务器，路由，中间件，OpenAPI |
| 数据库 & ORM | DAO，事务，查询 |
| CLI自动化 | gf-cli 命令 |
| 微服务 | gRPC，服务发现 |
| 部署 | 独立部署，Docker，Kubernetes |
| FAQ | 常见问题和解决方案 |

## 使用方法

将 `00-instructions.md` 的内容包含在 AI 助手的系统提示中，或作为规则文件引用。

## 相关项目

- [gf-skills](https://github.com/zhanghoufu/gf-skills/) - 综合知识库 (9个参考文件)
- [gf-mcp](https://github.com/zhanghoufu/gf-mcp/) - MCP 执行工具

## 许可证

MIT
