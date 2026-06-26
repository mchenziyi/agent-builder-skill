# Step 5.4 — 实现检索流程

```
用户查询 → Query Rewrite
  ├─ 向量检索（语义）
  ├─ 关键词检索（BM25）
  └─ RRF 合并 → Reranker → top-3
```

**RRF 合并：**
```python
def rrf_merge(results_list, k=60):
    scores = {}
    for results in results_list:
        for rank, doc_id in enumerate(results):
            scores[doc_id] = scores.get(doc_id, 0) + 1 / (k + rank)
    return sorted(scores.items(), key=lambda x: -x[1])
```
