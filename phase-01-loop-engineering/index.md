# Phase 1: Loop Engineering

> 设计 Agent 的主循环范式。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 1.1 | [step-1-1-three-paradigms.md](step-1-1-three-paradigms.md) | 理解三种核心范式 |
| 1.2 | [step-1-2-choose-paradigm.md](step-1-2-choose-paradigm.md) | 根据场景选择主范式 |
| 1.3 | [step-1-3-loop-skeleton.md](step-1-3-loop-skeleton.md) | 实现循环骨架 |
| 1.4 | [step-1-4-termination.md](step-1-4-termination.md) | 设计终止条件 |
| 1.5 | [step-1-5-fallback.md](step-1-5-fallback.md) | 实现 Fallback 策略 |

## 依赖

- 前置：无
- 被依赖：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — 完整闭环](verification.md#v1)
- [V2 — 终止条件](verification.md#v2)
- [V3 — 降级策略](verification.md#v3)

## 输出产物

- Loop Engine 模块
- 终止条件模块
- Fallback 降级模块
