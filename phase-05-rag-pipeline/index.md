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

## 协议规范

### "不要过度建设"判断准则

| 场景 | 建议 | 理由 |
|---|---|---|
| < 100 篇文档，单用户 | 文件读取 + BM25 | 向量库增加运维无收益 |
| 100-10000 篇文档 | Chroma + BGE | 轻量，够用 |
| > 10000 篇文档 | Milvus / Qdrant | 需要分布式和索引优化 |
| 已有 PostgreSQL | pgvector | 不引入额外中间件 |

### Chunking 约束
- chunk_size: 512-1024 tokens
- overlap: 50-100 tokens
- 必须在段落/句子边界切割，不允许在单词中间切断
- 同一文档的所有 chunk 共享一个 `doc_id`

### 检索约束
- top-K 默认 5，最多 20
- 当 Reranker 不可用时，至少使用向量 + 关键词双路召回
- 检索超时 2s，超时返回已返回的结果（部分结果优于无结果）

### Query Rewrite 约束
- 仅当原始查询结果 < 3 条时触发，不提前重写
- HyDE：生成假设答案的模型与最终问答模型可用不同模型（低成本模型生成假设）

## 依赖

- 前置：[Phase 2](../phase-02-core-architecture/index.md) + [Phase 4](../phase-04-tool-system/index.md)

## 验证标准

- [V1 — Chunking 质量](verification.md#v1)
- [V2 — 检索召回率](verification.md#v2)
- [V3 — 端到端问答](verification.md#v3)

## 输出产物

- Chunker 文档切割模块 / Embedder 模块 / Retriever 模块 / Query Rewriter 模块 / Evaluator 模块
