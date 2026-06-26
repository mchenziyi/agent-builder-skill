# Phase 3: Memory System

> 设计三层记忆架构。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 3.1 | [step-3-1-three-tiers.md](step-3-1-three-tiers.md) | 设计三层记忆架构 |
| 3.2 | [step-3-2-short-term.md](step-3-2-short-term.md) | 实现短期记忆 |
| 3.3 | [step-3-3-long-term.md](step-3-3-long-term.md) | 实现长期记忆 |
| 3.4 | [step-3-4-compression.md](step-3-4-compression.md) | 实现记忆压缩 |
| 3.5 | [step-3-5-granularity.md](step-3-5-granularity.md) | 设计读写粒度 |

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md)

## 验证标准

- [V1 — 短期记忆滑动窗口](verification.md#v1)
- [V2 — 长期记忆检索](verification.md#v2)
- [V3 — 压缩效果](verification.md#v3)

## 输出产物

- 记忆系统接口模块
- 短期记忆模块
- 长期记忆模块
- 压缩引擎模块
