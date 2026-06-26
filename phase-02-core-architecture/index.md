# Phase 2: Core Architecture

> 搭建 Agent 骨架。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 2.1 | [step-2-1-four-components.md](step-2-1-four-components.md) | 明确四大核心组件 |
| 2.2 | [step-2-2-three-layers.md](step-2-2-three-layers.md) | 区分三层职责 |
| 2.3 | [step-2-3-system-prompt.md](step-2-3-system-prompt.md) | 设计 System Prompt |
| 2.4 | [step-2-4-tool-registry.md](step-2-4-tool-registry.md) | 实现 Tool Registry |
| 2.5 | [step-2-5-session.md](step-2-5-session.md) | 设计 Session 管理 |
| 2.6 | [step-2-6-provider.md](step-2-6-provider.md) | 实现 Provider 抽象层 |

## 依赖

- 前置：[Phase 1](../phase-01-loop-engineering/index.md)
- 被依赖：Phase 3、Phase 4、Phase 5、Phase 7

## 验证标准

- [V1 — 四大组件](verification.md#v1)
- [V2 — Tool Registry](verification.md#v2)
- [V3 — Provider failover](verification.md#v3)

## 输出产物

- Profile / Prompt 管理模块
- Tool Registry 模块
- Session 管理模块
- Provider 抽象层模块
