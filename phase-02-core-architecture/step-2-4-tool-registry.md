# Step 2.4 — 实现 Tool Registry

Tool 的数据结构：

```
Tool {
    name: string          // 唯一标识
    description: string   // 何时调用此工具
    parameters: {         // JSON Schema
        type: "object",
        properties: { ... },
        required: [...]
    }
    handler: function     // 实际执行函数
}

registry = NewToolRegistry()
registry.register(Tool(
    name: "get_weather",
    description: "查询指定城市的实时天气",
    handler: weatherAPI
))
```

**三个能力：** 注册（name→handler）、发现（输出 JSON Schema）、调用（校验参数→执行→格式化结果）。
