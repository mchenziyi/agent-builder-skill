# Step 4.3 — 实现并行工具调用

> 无依赖的工具可同时执行，减少轮次。

## 触发条件

当用户问题涉及多个独立工具 / 同一工具不同参数时，模型可一次输出多个 tool_calls：

```
用户: "帮我查北京和上海的天气"
模型输出:
  tool_calls=[
    {id:"call_1", name:"get_weather", arguments:{"city":"北京"}},
    {id:"call_2", name:"get_weather", arguments:{"city":"上海"}}
  ]
```

## 并行执行要求

AI 必须使用目标项目原生并发机制实现：

| 语言 | 并发机制 |
|---|---|
| Python | `asyncio.gather` |
| JavaScript / TypeScript | `Promise.all` |
| Go | goroutine + `sync.WaitGroup` |
| Rust | `futures::future::join_all` |
| Java | `CompletableFuture.allOf` |

## 可并行 vs 不可并行

| | 可并行 | 不可并行 |
|---|---|---|
| 示例 | 查多个城市天气 | 先查订单号，再用订单号查物流 |
| 特征 | 无数据依赖 | 后一个工具依赖前一个的输出 |
| 模型行为 | 一轮输出多个 tool_calls | 分多轮串行输出 |
