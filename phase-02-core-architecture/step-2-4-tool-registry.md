# Step 2.4 — 实现 Tool Registry

```go
type Tool struct {
    Name        string
    Description string
    Parameters  map[string]Param
    Handler     func(args map[string]any) (any, error)
}

registry := NewToolRegistry()
registry.Register(Tool{
    Name: "get_weather",
    Description: "查询指定城市的实时天气",
    Handler: weatherAPI,
})
```

**三个能力：** 注册（name→handler）、发现（JSON Schema）、调用（校验+执行+格式化）。
