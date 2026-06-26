# Phase 5: RAG Pipeline

> 构建知识检索链路，让 Agent 利用外部知识库。

## 步骤

| # | 文件 | 说明 |
|---|---|---|
| 5.1 | [step-5-1-chunking.md](step-5-1-chunking.md) | 设计 Chunking 策略 |
| 5.2 | [step-5-2-embedding.md](step-5-2-embedding.md) | 选型 Embedding 模型 |
| 5.3 | [step-5-3-vector-db.md](step-5-3-vector-db.md) | 搭建向量数据库 |
| 5.4 | [step-5-4-retrieval.md](step-5-4-retrieval.md) | 实现检索流程 |
| 5.5 | [step-5-5-query-rewrite.md](step-5-5-query-rewrite.md) | 实现 Query Rewrite |
| 5.6 | [step-5-6-advanced-rag.md](step-5-6-advanced-rag.md) | 设计高级 RAG 范式 |
| 5.7 | [step-5-7-evaluation.md](step-5-7-evaluation.md) | 评估 RAG 效果 |

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md) + [Phase 4](../phase-04-tool-system/index.md)

## 验证标准

- [V1 — Chunking 质量](verification.md#v1)
- [V2 — 检索召回率](verification.md#v2)
- [V3 — 端到端问答](verification.md#v3)

## 输出产物

- Chunker 文档切割模块 / Embedder 模块 / Retriever 模块 / Query Rewriter 模块 / Evaluator 模块
