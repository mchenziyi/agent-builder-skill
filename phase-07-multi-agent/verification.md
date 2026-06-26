# Phase 7 验证细节

---

## V1 — 路由准确率

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 配置两个 Agent：一个处理天气查询，一个处理计算 | — |
| 2 | 发消息"北京天气" | 路由到天气 Agent |
| 3 | 发消息"123*456" | 路由到计算 Agent |
| 4 | 测试 20 条不同请求 | 路由准确率 > 95% |

- [ ] 路由能正确分发
- [ ] 模糊请求也有合理路由

## V2 — Agent 协作

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 设计任务：先查用户信息再推荐商品 | — |
| 2 | 用户 Agent 查信息 → 将结果传给推荐 Agent | — |
| 3 | 检查最终推荐 | 基于用户信息的个性化推荐 |

- [ ] Agent A 的输出正确传递给 Agent B
- [ ] 最终结果综合了多个 Agent 的能力

## V3 — 错误隔离

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 让 Agent A 崩溃（如关掉其依赖的 API） | — |
| 2 | 发送需要 Agent B 处理的任务 | Agent B 正常工作 |
| 3 | 发送需要 Agent A 处理的任务 | 协调器返回 "该服务暂时不可用" |

- [ ] 单 Agent 故障不影响其他 Agent
## 可执行验证

```go
// test_multi_agent_test.go
func TestRouting(t *testing.T) {
    router := NewRouter()
    router.AddRule("weather", "天气")
    router.AddRule("calc", "计算")
    
    agent := router.Route("北京天气")
    if agent != "weather" { t.Error("路由错误") }
    
    agent = router.Route("123*456")
    if agent != "calc" { t.Error("路由错误") }
}

func TestAgentCommunication(t *testing.T) {
    coord := NewOrchestrator()
    coord.RegisterAgent("agent_a", &mockAgent{name: "a"})
    coord.RegisterAgent("agent_b", &mockAgent{name: "b"})
    
    result := coord.Execute(Plan{
        Steps: []Step{
            {Agent: "agent_a", Task: "获取用户信息"},
            {Agent: "agent_b", Task: "根据用户信息推荐"},
        }
    })
    if result.Error != nil { t.Error("协作失败") }
}

func TestCircuitBreaker(t *testing.T) {
    cb := NewCircuitBreaker(WithThreshold(3))
    for i := 0; i < 5; i++ {
        cb.Call(func() error { return errors.New("fail") })
    }
    if cb.State() != "open" { t.Error("熔断器未打开") }
}
```
