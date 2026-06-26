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

## 前置条件

- 循环骨架已实现（Phase 1）
- 目标项目已有基础数据结构定义

## AI 必须产出

- Tool Registry 模块（注册 + 发现 + 调用）
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：`Register(name, description, parameters, handler)`、`List()`、`Execute(name, args)`
- **输出**：List 返回 JSON Schema 数组；Execute 返回工具执行结果
- **错误**：注册重复 name 返回错误；调用不存在的 name 返回 NotFound
- **不变量**：已注册的工具不可被同 name 覆盖（先注销再注册除外）

## 完成定义

- 至少注册 3 个工具，List 返回正确
- Execute 对已知工具返回正确结果，对未知工具返回错误
- 对应 verification.md V2 测试通过
