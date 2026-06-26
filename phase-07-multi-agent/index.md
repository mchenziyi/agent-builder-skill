# Phase 7: Multi-Agent

> 多 Agent 协作与通信。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 7.1 | [step-7-1-when-to-upgrade.md](step-7-1-when-to-upgrade.md) | 判断升级时机 |
| 7.2 | [step-7-2-routing.md](step-7-2-routing.md) | 设计路由机制 |
| 7.3 | [step-7-3-communication.md](step-7-3-communication.md) | 实现 Agent 间通信 |
| 7.4 | [step-7-4-orchestrator.md](step-7-4-orchestrator.md) | 设计协调器 |
| 7.5 | [step-7-5-isolation.md](step-7-5-isolation.md) | 实现错误隔离 |

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md) + [Phase 4](../phase-04-tool-system/index.md)

## 验证标准

- [V1 — 路由准确率](verification.md#v1)
- [V2 — Agent 协作](verification.md#v2)
- [V3 — 错误隔离](verification.md#v3)

## 输出产物

- Orchestrator 协调器模块 / Router 路由模块 / Agent 通信协议模块 / Circuit Breaker 熔断器模块
