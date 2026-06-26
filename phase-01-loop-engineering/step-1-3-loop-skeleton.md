# Step 1.3 — 实现循环骨架

核心循环的伪代码：

```
function agent_loop(user_input, tools):
    messages = [system_prompt, user_message(user_input)]
    rounds = 0

    while rounds < MAX_ROUNDS:
        rounds++
        response = llm.chat(messages, tools)

        if response.finish_reason == "stop":
            return response.content

        elif response.finish_reason == "tool_calls":
            for each tool_call in response.tool_calls:
                result = execute_tool(tool_call.name, tool_call.arguments)
                messages.append(tool_message(tool_call.id, result))

    return fallback("已达最大轮次")
```

**实现方式：** 状态机（精细控制）/ while 循环（简单快速）/ 事件驱动（高并发）。
