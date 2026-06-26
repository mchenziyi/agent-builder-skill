# Step 7.2 — 设计路由机制

```go
func Route(ctx context.Context, req Request) AgentID {
    if agent := matchByRule(req); agent != "" {
        return agent  // 规则匹配（零延迟）
    }
    agent, err := llmRouter.SelectAgent(ctx, req)
    if err != nil {
        return DefaultAgent
    }
    return agent
}
```

**混合路由：** 规则路由（快）+ LLM 路由（灵活）+ 兜底。
