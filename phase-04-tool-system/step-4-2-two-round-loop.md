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
