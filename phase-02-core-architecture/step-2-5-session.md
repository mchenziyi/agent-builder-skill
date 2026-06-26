# Step 2.5 — 设计 Session 管理

Session 的数据结构：

```
Session {
    id: string          // 全局唯一（UUID / ULID）
    messages: []        // 对话历史
    metadata: {}        // 业务元数据
}

SessionStore 接口：
    get(id) → Session | NotFound
    save(session)
    delete(id)
```

**存储选型：** 原型用内存，生产用 Redis 或数据库。
