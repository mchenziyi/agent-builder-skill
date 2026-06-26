# Step 7.2 — 设计路由机制

```
function route(request):
    // 1. 先试规则匹配（零延迟）
    agent = match_by_rule(request)
    if agent != null:
        return agent

    // 2. 规则匹配不到 → LLM 动态路由
    agent = llm_router.select_agent(request)
    if agent == null:
        return default_agent
    return agent
```

**混合路由：** 规则路由（快）+ LLM 路由（灵活）+ 兜底默认 Agent。
