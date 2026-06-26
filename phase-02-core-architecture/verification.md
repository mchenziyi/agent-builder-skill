# Phase 2 验证细节

---

## V1 — 四大组件

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 定义 Profile / Memory / Planning / Action 四个接口 | 职责清晰，无重叠 |
| 2 | 实现至少一个组件 | 实现完整 |
| 3 | 编写组件间调用测试 | 组件可互相调用 |

- [ ] 四个接口定义完毕
- [ ] 组件间调用链路正常

## V2 — Tool Registry

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 注册 3 个不同工具 | 全部注册成功 |
| 2 | 调用 ListTools() | 返回 3 个工具的 JSON Schema |
| 3 | 调用 Execute("get_weather", {"city":"北京"}) | 返回正确结果 |
| 4 | 调用不存在的工具 | 返回错误，不崩溃 |

- [ ] 注册/发现/调用全链路正常
- [ ] 非法调用有错误处理

## V3 — Provider Failover

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 配置两个 Provider，第二个设无效 API Key | — |
| 2 | 发送聊天请求 | 自动切换到可用 Provider |
| 3 | 检查响应 | 正常返回 |

## 可执行验证

```go
// test_registry_test.go
func TestRegistry(t *testing.T) {
    r := NewToolRegistry()
    
    // V1：注册三个工具
    r.Register(Tool{Name: "a", Handler: func(a any) (any, error) { return "a", nil }})
    r.Register(Tool{Name: "b", Handler: func(a any) (any, error) { return "b", nil }})
    r.Register(Tool{Name: "c", Handler: func(a any) (any, error) { return "c", nil }})
    
    // V2：发现
    tools := r.List()
    if len(tools) != 3 { t.Error("V2: 工具数量不符") }
    
    // V3：调用
    result, err := r.Execute("a", nil)
    if err != nil || result != "a" { t.Error("V3: 调用失败") }
    
    // V4：不存在的工具
    _, err = r.Execute("not_exists", nil)
    if err == nil { t.Error("V4: 预期错误未返回") }
}

// test_provider_test.go
func TestProviderFailover(t *testing.T) {
    pm := NewProviderManager()
    pm.AddProvider(&mockProvider{name: "main", available: true})
    pm.AddProvider(&mockProvider{name: "backup", available: false})
    
    resp, err := pm.Chat("test")
    if err != nil { t.Error("failover 失败") }
}
```
