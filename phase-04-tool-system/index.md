# Phase 4: Tool System

> 让 Agent 具备工具调用能力。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 4.1 | [step-4-1-json-schema.md](step-4-1-json-schema.md) | 设计 JSON Schema 工具描述 |
| 4.2 | [step-4-2-two-round-loop.md](step-4-2-two-round-loop.md) | 实现两轮对话闭环 |
| 4.3 | [step-4-3-parallel-calls.md](step-4-3-parallel-calls.md) | 实现并行工具调用 |
| 4.4 | [step-4-4-mcp.md](step-4-4-mcp.md) | 接入 MCP 协议 |
| 4.5 | [step-4-5-skill.md](step-4-5-skill.md) | 构建 Skill 封装层 |
| 4.6 | [step-4-6-security.md](step-4-6-security.md) | 设计安全边界 |

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — FC 基本调用](verification.md#v1)
- [V2 — 工具结果回填](verification.md#v2)
- [V3 — 并行调用](verification.md#v3)
- [V4 — MCP 接入](verification.md#v4)
- [V5 — 安全权限](verification.md#v5)
- [V6 — 错误降级](verification.md#v6)

## 输出产物

- Tool Registry 模块
- Tool Executor 模块
- MCP Client 模块
- Skill Loader 模块
- Security 校验模块
