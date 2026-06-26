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

## 前置条件

- 已选定主循环范式（step-1-2）
- 目标项目已初始化，可编译运行

## AI 必须产出

- 循环骨架模块（含 think → act → observe 主循环）
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：用户消息字符串，工具列表
- **输出**：Agent 回复字符串
- **错误**：达到最大轮次后返回 fallback 消息而非 panic
- **不变量**：每次循环后 `messages` 长度增长（至少追加一条助手消息或 tool 结果）

## 完成定义

- 循环骨架可编译通过
- 通过 mock provider 可跑通一次完整的 think → act → observe（见 verification.md V1）
- 达到 MAX_ROUNDS 后正确终止（见 verification.md V2）
