# Phase 1 验证细节

---

## V1 — 完整闭环

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 实现最简循环骨架（while + 条件判断） | 编译通过 |
| 2 | 注册一个简单工具（如 `echo`，返回输入内容） | 注册成功 |
| 3 | 发送消息："你好" | 模型正常回复 |
| 4 | 发送消息："调用 echo 工具，内容是 hello" | 模型输出 tool_calls |
| 5 | 执行工具并回填结果 | 成功 |
| 6 | 检查最终回复 | 基于工具结果生成 |

- [ ] 完整的 think → act → observe 闭环跑通
- [ ] 工具调用结果正确回填

## V2 — 终止条件

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 设 MAX_ROUNDS = 2 | — |
| 2 | 发送需要 3 轮才能完成的任务 | 第 2 轮后终止，返回 fallback |
| 3 | 检查日志 | 记录了终止原因 |

- [ ] 超轮次后正确终止
- [ ] 返回友好的 fallback 消息

## V3 — 降级策略

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 让工具连续返回错误 | — |
| 2 | 触发该工具 3 次 | 第 3 次后 Agent 降级 |
| 3 | 检查后续行为 | 不再尝试调工具 |

## 可执行验证（V1）

```go
// test_loop_complete_test.go
package agent

import "testing"

func TestLoopComplete(t *testing.T) {
    agent := NewAgent()
    agent.RegisterTool("echo", func(args map[string]any) (any, error) {
        return args["msg"], nil
    })

    // V1.1：发送简单消息，期望直接回复
    resp := agent.Chat("你好")
    if resp == "" {
        t.Error("V1.1: Agent 未回复")
    }

    // V1.2：发送需要调工具的消息
    resp = agent.Chat("调用 echo 工具，内容 hello")
    if !resp.Contains("hello") {
        t.Error("V1.2: Agent 未基于工具结果回复")
    }
    t.Log("✅ V1 完整闭环通过")
}
```

## 可执行验证（V2）

```go
func TestTermination(t *testing.T) {
    agent := NewAgent(WithMaxRounds(2))
    agent.RegisterTool("slow_tool", func(args map[string]any) (any, error) {
        return "done", nil
    })

    // 发一条需要 3 轮才能完成的任务
    resp := agent.Chat("连续调用 3 次 slow_tool")
    if resp.Contains("超时") || resp.Contains("已达最大轮次") {
        t.Log("✅ V2 终止条件正常触发")
    } else {
        t.Error("V2: 未触发终止条件")
    }
}
```

## 可执行验证（V3）

```go
func TestFallback(t *testing.T) {
    agent := NewAgent()
    callCount := 0
    agent.RegisterTool("broken_tool", func(args map[string]any) (any, error) {
        callCount++
        return nil, errors.New("故意失败")
    })

    resp := agent.Chat("调 broken_tool 三次")
    if callCount >= 3 && !resp.Contains("error") {
        t.Log("✅ V3 降级策略生效")
    } else {
        t.Error("V3: 降级未触发")
    }
}
```
