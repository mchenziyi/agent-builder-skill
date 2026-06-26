---
name: agent-builder
description: Builds an Agent system phase by phase. Use when designing or implementing Agent loops, tool systems, memory, RAG, multi-agent orchestration, or harness/evaluation systems.
---

# build-agent-system

让任意 AI 按步骤搭建一个功能完整的 Agent 系统。从主循环到进化机制，8 个 Phase 覆盖全链路。

## 执行规则

- **按序执行**：Phase 1→2→3→...→8，前一个验证通过后进入下一个
- **每步必验**：每个 Phase 完成后执行 `verification.md`，未通过需修复
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

| Phase | 步骤 | 产出 |
|---|---|---|
| [1. Loop Engineering](phase-01-loop-engineering/index.md) | 5 | `loop_engine.go`, `termination.go`, `fallback.go` |
| [2. Core Architecture](phase-02-core-architecture/index.md) | 6 | `registry.go`, `session.go`, `provider.go` |
| [3. Memory System](phase-03-memory-system/index.md) | 5 | `short_term.go`, `long_term.go`, `compressor.go` |
| [4. Tool System](phase-04-tool-system/index.md) | 6 | `tool_registry.go`, `mcp_client.go`, `skill_loader.go` |
| [5. RAG Pipeline](phase-05-rag-pipeline/index.md) | 7 | `chunker.go`, `retriever.go`, `evaluator.go` |
| [6. Model Optimization](phase-06-model-optimization/index.md) | 5 | `params.go`, `model_benchmark.go` |
| [7. Multi-Agent](phase-07-multi-agent/index.md) | 5 | `orchestrator.go`, `router.go`, `circuit_breaker.go` |
| [8. Harness Engineering](phase-08-harness-engineering/index.md) | 5 | `feedback.go`, `prompt_optimizer.go`, `dashboard.go` |
