---
name: agent-builder
description: Builds an Agent system phase by phase. Use when designing or implementing Agent loops, tool systems, memory, RAG, multi-agent orchestration, or harness/evaluation systems.
---

# build-agent-system

让任意 AI 按步骤搭建一个功能完整的 Agent 系统。从主循环到进化机制，8 个 Phase 覆盖全链路。

## 执行规则

- **按序执行**：Phase 1→2→3→...→8，前一个验证通过后进入下一个
- **每 Phase 必验**：每个 Phase 完成后执行 `verification.md`，未通过需修复
- **按需深读**：读 `index.md` 了解概览 → 执行单个 step-*.md → 完成读 `verification.md`
- **MVP 优先**：Phase 1→2→4 可先跑通最小 Agent

## 依赖关系

```
Phase 1 ← 无前置
Phase 2 ← Phase 1
Phase 3 ← Phase 2
Phase 4 ← Phase 2
Phase 5 ← Phase 2 + Phase 4
Phase 6 ← 可并行
Phase 7 ← Phase 2 + Phase 4
Phase 8 ← Phase 1-7
```

## Phase 列表

| Phase | 步骤 | 产出（示例命名，按目标语言调整） |
|---|---|---|
| [1. Loop Engineering](phase-01-loop-engineering/index.md) | 5 | Loop Engine 模块、终止条件、Fallback 策略 |
| [2. Core Architecture](phase-02-core-architecture/index.md) | 6 | Tool Registry、Session 管理、Provider 抽象层 |
| [3. Memory System](phase-03-memory-system/index.md) | 5 | 短期记忆、长期记忆、压缩引擎 |
| [4. Tool System](phase-04-tool-system/index.md) | 6 | 工具注册中心、MCP 客户端、Skill 加载器 |
| [5. RAG Pipeline](phase-05-rag-pipeline/index.md) | 7 | 文档切割器、检索器、效果评估 |
| [6. Model Optimization](phase-06-model-optimization/index.md) | 5 | 解码参数配置、模型评测脚本 |
| [7. Multi-Agent](phase-07-multi-agent/index.md) | 5 | 协调器、路由模块、熔断器 |
| [8. Harness Engineering](phase-08-harness-engineering/index.md) | 5 | 反馈采集、Prompt 优化器、评估看板 |
