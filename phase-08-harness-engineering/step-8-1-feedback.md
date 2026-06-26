# Step 8.1 — 设计反馈采集

```
Feedback {
    session_id: string
    type: "explicit" | "implicit" | "execution" | "correction"
    score: float      // 0.0 - 1.0
    detail: string
}

FeedbackStore 接口：
    record(feedback)
    query(filter) → Feedback[]
    get_stats(time_range) → Stats
```

| 类型 | 采集 | 质量 | 覆盖度 |
|---|---|---|---|
| 显式 | 赞/踩 | 高 | 低 |
| 隐式 | 用户修改输出 | 中 | 高 |
| 执行 | 工具成功/失败 | 高 | 高 |
