# Phase 6: Model Optimization

> 优化推理性能、降低成本、模型选型。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 6.1 | [step-6-1-caching.md](step-6-1-caching.md) | 实现 Prompt Caching / KV Cache |
| 6.2 | [step-6-2-params.md](step-6-2-params.md) | 配置解码参数 |
| 6.3 | [step-6-3-quantization.md](step-6-3-quantization.md) | 评估量化方案 |
| 6.4 | [step-6-4-framework.md](step-6-4-framework.md) | 对比推理框架 |
| 6.5 | [step-6-5-model-selection.md](step-6-5-model-selection.md) | 模型选型评估 |

## 依赖

- 前置：无（可并行）

## 验证标准

- [V1 — Prompt Caching 效果](verification.md#v1)
- [V2 — 解码参数调优](verification.md#v2)
- [V3 — 选型评估](verification.md#v3)

## 输出产物

- 解码参数配置模块 / 模型评测模块 / 选型报告
