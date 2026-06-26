# Step 8.3 — 实现 Skill 迭代

自动检测高频工具调用序列，提取为 Skill：

```
检测：get_user_info 后 83% 紧接着 get_user_orders
  → 生成 Skill "用户信息查询"
  → 步骤：[get_user_info, get_user_orders]
  → 验证后注册
```

**流水线：** 统计 → 提取 → 验证 → 注册 → 监控。
