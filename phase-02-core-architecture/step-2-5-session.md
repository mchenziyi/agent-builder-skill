# Step 2.5 — 设计 Session 管理

```go
type Session struct {
    ID        string
    Messages  []Message
    Metadata  map[string]any
}

type SessionStore interface {
    Get(id string) (*Session, error)
    Save(s *Session) error
    Delete(id string) error
}
```

**存储选型：** 原型用内存，生产用 Redis 或数据库。
