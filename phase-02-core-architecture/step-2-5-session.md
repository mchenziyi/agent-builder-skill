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

## 前置条件

- Tool Registry 已实现（step-2-4）
- 目标项目已确定消息结构的序列化格式（JSON）

## AI 必须产出

- Session 管理模块
- 文件名按目标项目语言和目录结构决定

## 最小接口契约

- **输入**：`Create()`、`Get(id)`、`Save(session)`、`Delete(id)`
- **输出**：Session 对象（含 ID、消息列表、元数据）
- **错误**：Get 不存在的 ID 返回 NotFound
- **不变量**：Session.ID 全局唯一（UUID/ULID）

## 完成定义

- Session 可创建、保存、恢复
- 序列化后反序列化得到相同对象
- 对应 verification.md V2 测试通过
