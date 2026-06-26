# Step 4.1 — 设计 JSON Schema 工具描述

> 每个工具用 JSON Schema 描述，模型据此决定「要不要调、参数怎么填」。

## 完整示例

```json
{
  "type": "function",
  "function": {
    "name": "get_weather",
    "description": "查询指定城市的实时天气，包含气温、天气状况、风向风速，仅支持中国大陆城市",
    "parameters": {
      "type": "object",
      "properties": {
        "city": {
          "type": "string",
          "description": "城市名称，如「北京」「上海」，不要带省份前缀"
        },
        "unit": {
          "type": "string",
          "enum": ["celsius", "fahrenheit"],
          "description": "温度单位，默认用摄氏度"
        }
      },
      "required": ["city"]
    }
  }
}
```

## 关键字段

| 字段 | 作用 | 最佳实践 |
|---|---|---|
| `name` | 工具唯一标识 | 小写 + 下划线，如 `get_user_info` |
| `description` | 模型判断是否调用的依据 | **最重要字段**，写清楚何时调用、输入输出示例 |
| `parameters.properties` | 每个参数的定义 | 写清楚格式要求、枚举值、边界条件 |
| `parameters.required` | 必填参数列表 | 只标记真正必需的 |

## Description 反例

```
❌ "获取天气" → 模型可能乱填参数
✅ "查询指定城市的实时天气，包含气温、天气状况、风向风速，仅支持中国大陆城市" → 准确调用
```

## 前置条件

- Tool Registry 已实现（step-2-4）
- 至少确定一个需要暴露给 Agent 的真实工具

## AI 必须产出

- 每个工具对应的 JSON Schema 定义
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：工具名称、功能描述、参数列表
- **输出**：符合目标模型 / Provider Function Calling 格式的 JSON Schema 对象
- **不变量**：每个工具必须有唯一的 name；description 不超过 1024 字符

## 完成定义

- 至少为一个真实工具编写了完整的 JSON Schema
- Schema 通过了目标模型的工具调用格式校验
- 对应 verification.md V1 测试通过
