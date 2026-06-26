# Step 7.5 — 实现错误隔离

```
CircuitBreaker {
    failures: int
    threshold: int              // 熔断阈值（默认 3）
    reset_timeout: duration     // 自动恢复时间
    state: "closed" | "open" | "half-open"
}
```

**隔离级别：** 进程级（最安全）> 协程级（轻量）> 沙箱级（中间方案）。
