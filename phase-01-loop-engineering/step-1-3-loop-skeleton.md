# Step 1.3 — 实现循环骨架

```go
func AgentLoop(userInput string, tools []Tool) string {
    messages := []Message{systemPrompt, {role: "user", content: userInput}}
    rounds := 0

    for {
        if rounds > MAX_ROUNDS {
            return fallbackResponse("已达最大轮次")
        }
        rounds++

        response := llm.Chat(messages, tools)

        switch response.FinishReason {
        case "stop":
            return response.Content
        case "tool_calls":
            for _, tc := range response.ToolCalls {
                result := executeTool(tc.Function.Name, tc.Function.Arguments)
                messages = append(messages, ToolResultMessage(tc.ID, result))
            }
        }
    }
}
```

**实现方式：** 状态机（精细控制）/ while 循环（简单快速）/ 事件驱动（高并发）。
