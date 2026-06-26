# Step 8.1 — 设计反馈采集

```go
type Feedback struct {
    SessionID string
    Type      string  // explicit / implicit / execution / correction
    Score     float32
    Detail    string
}
```

| 类型 | 采集 | 质量 | 覆盖度 |
|---|---|---|---|
| 显式 | 赞/踩 | 高 | 低 |
| 隐式 | 用户修改输出 | 中 | 高 |
| 执行 | 工具成功/失败 | 高 | 高 |
