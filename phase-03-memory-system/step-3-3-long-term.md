# Step 3.3 — 实现长期记忆

```go
type MemoryItem struct {
    ID         string
    Content    string
    Embedding  []float32
    Importance float32
    CreatedAt  time.Time
}
```

**存储：** 提取关键信息 → Embedding → 存入向量库
**检索：** 用户输入 → Embedding → 向量检索 top-5 → 去重 → 注入上下文
