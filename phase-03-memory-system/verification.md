# Phase 3 验证细节

---

## V1 — 短期记忆滑动窗口

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 设滑动窗口 = 3 条消息 | — |
| 2 | 发 5 条消息 | 第 6 条时最早的 1、2 条被丢弃 |
| 3 | 检查当前 messages 长度 | ≤ 3 条 |

- [ ] 滑动窗口正确工作
- [ ] 最早消息被丢弃

## V2 — 长期记忆检索

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 存储一条记忆："用户喜欢蓝色" | 存入成功 |
| 2 | 新会话中问"我喜欢什么颜色" | 检索到相关记忆 |
| 3 | 检查回复 | 包含"蓝色" |

- [ ] 跨会话能检索到历史记忆
- [ ] 检索结果与用户问题相关

## V3 — 压缩效果

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 发起 15 轮对话 | — |
| 2 | 触发压缩 | 成功压缩 |
| 3 | 检查压缩前后的 Token 数 | 压缩后 < 压缩前 50% |
| 4 | 检查压缩后关键信息是否保留 | 关键事实（用户名字、偏好）未丢失 |

- [ ] Token 消耗显著下降
## 可执行验证

```go
func TestShortTermMemory(t *testing.T) {
    m := NewShortTermMemory(WithMaxMessages(3))
    
    m.Add("msg1"); m.Add("msg2"); m.Add("msg3")
    if len(m.Messages()) != 3 { t.Error("初始 3 条") }
    
    m.Add("msg4")  // 应丢弃 msg1
    if len(m.Messages()) != 3 { t.Error("滑动窗口应保持 3 条") }
    if m.Messages()[0] == "msg1" { t.Error("最早的 msg1 应被丢弃") }
}

func TestLongTermMemory(t *testing.T) {
    m := NewLongTermMemory()
    m.Store("用户喜欢蓝色")
    
    results := m.Search("我喜欢什么颜色")
    if !contains(results, "蓝色") { t.Error("长期记忆检索失败") }
}

func TestCompression(t *testing.T) {
    m := NewMemoryManager()
    for i := 0; i < 15; i++ {
        m.Add(fmt.Sprintf("msg%d", i))
    }
    before := m.TokenCount()
    m.Compress()
    after := m.TokenCount()
    if after >= before { t.Error("压缩后 Token 未减少") }
}
```
