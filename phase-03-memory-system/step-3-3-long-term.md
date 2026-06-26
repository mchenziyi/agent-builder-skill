# Step 3.3 — 实现长期记忆

记忆项的数据结构：

```
MemoryItem {
    id: string
    content: string
    embedding: float[]      // 向量
    importance: float       // 0-1
    created_at: timestamp
}
```

**存储：** 提取关键信息 → Embedding → 存入向量库
**检索：** 用户输入 → Embedding → 向量检索 top-5 → 去重 → 注入上下文
