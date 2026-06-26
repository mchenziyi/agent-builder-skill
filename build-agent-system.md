# build-agent-system

> 完整 Agent 构建指南 — 从主循环到进化机制，8 个 Phase 按序执行。

## 核心原则

- **默认完整路径**：按 Phase 1→2→3→...→8 执行，前一个验证通过后才进下一个
- **MVP 路径例外**：当用户要求 MVP / 最小闭环 / 快速跑通时，允许先执行 Phase 1→2→4，跳过 Phase 3，待工具闭环完成后再补 Phase 3、5、6、7、8
- 每个 Phase 完成后必须跑 verification.md，未通过需修复
- MVP 路径优先：Phase 1 → 2 → 4 先跑通最小闭环

## Phase 依赖关系

```
Phase 1 ← 无前置
Phase 2 ← Phase 1
Phase 3 ← Phase 2
Phase 4 ← Phase 2
Phase 5 ← Phase 2 + Phase 4
Phase 6 ← Phase 2 Provider 抽象完成后可并行；真实评测/成本优化需评测集和日志/trace
Phase 7 ← Phase 2 + Phase 4
Phase 8 ← Phase 1-7
```

## MVP 路径

```
Phase 1 → Phase 2 → Phase 4 → 最小 Agent → 后续逐步完善
```

## Phase 列表

| Phase | 说明 | 步骤 |
|---|---|---|
| [1. Loop Engineering](phase-01-loop-engineering/index.md) | 设计主循环范式 | 5 步 |
| [2. Core Architecture](phase-02-core-architecture/index.md) | 搭建 Agent 骨架 | 6 步 |
| [3. Memory System](phase-03-memory-system/index.md) | 记忆模块设计 | 5 步 |
| [4. Tool System](phase-04-tool-system/index.md) | 工具调用系统 | 6 步 |
| [5. RAG Pipeline](phase-05-rag-pipeline/index.md) | 知识检索链路 | 7 步 |
| [6. Model Optimization](phase-06-model-optimization/index.md) | 引擎优化 | 5 步 |
| [7. Multi-Agent](phase-07-multi-agent/index.md) | 多 Agent 协作 | 5 步 |
| [8. Harness Engineering](phase-08-harness-engineering/index.md) | 进化机制 | 5 步 |
