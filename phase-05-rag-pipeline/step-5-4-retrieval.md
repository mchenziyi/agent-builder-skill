# Step 5.4 — 实现检索流程

```
用户查询 → Query Rewrite
  ├─ 向量检索（语义）
  ├─ 关键词检索（BM25）
  └─ RRF 合并 → Reranker → top-3
```

**RRF 合并：**

```
function rrf_merge(results_list, k=60):
    scores = {}
    for each results in results_list:
        for rank, doc_id in enumerate(results):
            scores[doc_id] += 1 / (k + rank)
    return sort(scores, descending)
```
