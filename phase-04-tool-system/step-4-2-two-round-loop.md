# Step 4.2 — 实现两轮对话闭环

## 两轮闭环契约

1. **第一轮**：调用 Provider，输入包含 `messages` 和 `tools`
2. 如果返回 `finish_reason = "tool_calls"`：
   - 校验每个 tool_call 包含 `name`、`arguments`（合法 JSON）、`id`
   - 调用 Tool Registry 执行对应工具
   - 将执行结果按目标模型协议回填（`role: "tool"`、`tool_call_id`）
3. **第二轮**：再次调用 Provider，输入追加了回填的 tool result
4. 如果返回 `finish_reason = "stop"` → 输出最终回复
5. 如果继续返回 `"tool_calls"` → 回到步骤 2，直到完成或达到最大轮次

## 必须验证

- 如果第二轮继续返回 `tool_calls`，循环必须继续执行，直到 `stop` 或达到最大轮次
- 每个 tool_call 都有对应的 tool result（tool_call_id 不丢失）
- 工具失败不会导致循环崩溃（返回错误而非 panic）
- 第二轮回复必须基于工具结果（非模型自行猜测）

## 流程图

```
用户 → "北京天气"
  ↓ 传入 messages + tools
[LLM]
  ↓ finish_reason="tool_calls", tool_calls=[...]
[代码执行工具]
  ↓ 结果
[回填 role="tool"]
  ↓
[LLM 再次调用]
  ↓ finish_reason="stop"
用户 ← 最终回复
```

## 前置条件

- Tool Registry 已实现（step-2-4）
- JSON Schema 工具描述已编写完毕（step-4-1）
- Provider 抽象层已实现（step-2-6）

## AI 必须产出

- 两轮对话调用模块（集成 Tool Registry + Provider）
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：用户消息、可用工具列表、当前会话
- **输出**：Agent 最终回复
- **错误**：工具执行失败回填结构化错误而非 panic
- **不变量**：每次 tool_calls 中的 tool_call_id 与回填的 tool 消息一一对应，不丢失不串错

## 完成定义

- 可跑通一轮完整的两轮对话（tool_calls → 执行 → 回填 → 回复）
- 第二轮继续返回 tool_calls 时循环正常继续
- 对应 verification.md V1、V2 测试通过
