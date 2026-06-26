# Step 7.3 — 实现 Agent 间通信

```go
type AgentMessage struct {
    From      AgentID
    To        AgentID
    Type      string  // request / response / broadcast
    Payload   map[string]any
    RequestID string
}
```

| 方案 | 优点 | 缺点 |
|---|---|---|
| 消息传递 | 解耦、可审计 | 格式需标准化 |
| 共享状态 | 一致性高 | 耦合高 |
| 混合 | 兼顾 | 实现最复杂 |
