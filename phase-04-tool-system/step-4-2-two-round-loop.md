# Step 4.2 — 实现两轮对话闭环

> 第一轮模型输出 tool_calls → 代码执行 → 第二轮模型给最终答案。

## 核心代码

```python
response = client.chat.completions.create(
    model="gpt-4o",
    messages=messages,
    tools=tools,
    tool_choice="auto"
)
msg = response.choices[0].message

if msg.finish_reason == "tool_calls":
    for tool_call in msg.tool_calls:
        func_args = json.loads(tool_call.function.arguments)
        result = execute_tool(tool_call.function.name, func_args)
        messages.append({
            "role": "tool",
            "tool_call_id": tool_call.id,
            "content": result
        })
    final = client.chat.completions.create(
        model="gpt-4o",
        messages=messages,
        tools=tools
    )
```

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
