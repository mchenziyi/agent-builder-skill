# Step 2.6 — 实现 Provider 抽象层

```go
type Provider interface {
    Name() string
    Chat(messages, tools) (*Response, error)
    IsAvailable() bool
}

// failover：主模型失败 → 切备用
func (pm *ProviderManager) Chat(req Request) (*Response, error) {
    resp, err := pm.providers[pm.activeIdx].Chat(req)
    if err == nil { return resp, nil }
    for _, p := range pm.providers {
        if p.IsAvailable() { return p.Chat(req) }
    }
    return nil, ErrAllFailed
}
```

**典型配置：** 1. 主力模型 → 2. 快速模型（降级）→ 3. 备用 Provider（保底）
