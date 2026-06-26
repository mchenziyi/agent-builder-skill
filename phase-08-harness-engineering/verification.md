# Phase 8 验证细节

---

## V1 — 反馈采集

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 模拟发送一次用户点赞 | 反馈被记录 |
| 2 | 模拟一次工具调用失败 | 执行反馈被记录 |
| 3 | 查询反馈统计 | 两条反馈都可查到 |

- [ ] 各种反馈类型都被正确采集
- [ ] 可查询和统计

## V2 — 自动优化闭环

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 准备一批失败案例（如 10 次工具调用失败） | — |
| 2 | 运行自动优化流程 | 生成至少 1 条改进建议 |
| 3 | 人工评估建议质量 | 建议合理可执行 |

- [ ] 自动优化能基于失败案例生成建议
- [ ] 建议质量可接受

## V3 — 改进趋势

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 运行评估看板 | 数据正确显示 |
| 2 | 应用一次改进（如优化 Prompt） | — |
| 3 | 运行 24 小时后对比指标 | 有改善趋势（如任务完成率提升） |

- [ ] 评估看板正常显示
- [ ] 改进后核心指标有积极变化
## 可执行验证

```go
// test_feedback_test.go
func TestFeedbackCollection(t *testing.T) {
    store := NewFeedbackStore()
    
    store.Record(Feedback{SessionID: "s1", Type: "explicit", Score: 0.8})
    store.Record(Feedback{SessionID: "s2", Type: "execution", Score: 0.0})
    
    stats := store.GetStats(TimeRange{Last: 24 * time.Hour})
    if stats.Total != 2 { t.Error("反馈采集数量不符") }
}

func TestPromptOptimizer(t *testing.T) {
    opt := NewPromptOptimizer()
    failures := []Failure{
        {Pattern: "误删文件", Prompt: "当前 prompt"},
        {Pattern: "误删文件", Prompt: "当前 prompt"},
    }
    
    suggestions := opt.Analyze(failures)
    if len(suggestions) == 0 { t.Error("未生成改进建议") }
    t.Logf("建议: %v", suggestions)
}
```

## 持续监控命令

```bash
# 查看最近 24h 的 Agent 健康指标
# go run dashboard.go --time=24h

# 输出示例：
# 任务完成率: 94.2%     ↑ 2.1%
# 平均轮次:    3.8      ↓ 0.4
# 用户满意度:  4.3      ↑ 0.2
# 工具成功率:  96.7%    ↑ 1.1%
```
