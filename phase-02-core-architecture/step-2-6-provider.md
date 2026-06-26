# Step 2.6 — 实现 Provider 抽象层

Provider 最低接口：

```
Provider {
    chat(messages, tools) → Response
    chat_stream(messages, tools) → Stream<Response>
    is_available() → bool
}

// failover：主模型失败 → 切备用
function chat_with_failover(request):
    for each provider in providers:
        if provider.is_available():
            return provider.chat(request)
    return error("所有 Provider 不可用")
```

**典型配置：** 1. 主力模型 → 2. 快速模型（降级）→ 3. 备用 Provider（保底）
