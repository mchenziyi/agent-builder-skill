# Step 8.2 — 实现自动 Prompt 优化

```python
def auto_optimize_prompt(failures):
    patterns = cluster_by_error_type(failures)
    for pattern in patterns:
        suggestion = llm.analyze(f"失败模式：{pattern}，请给出改进建议")
        candidates.append(apply_suggestion(current_prompt, suggestion))
    return ab_test(candidates)
```

**流程：** 收集失败 → 聚类模式 → 生成改进 → A/B 测试 → 上线。
