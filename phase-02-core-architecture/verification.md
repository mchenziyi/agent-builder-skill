# Phase 2 验证细节

---

## V1 — 四大组件

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 定义 Profile / Memory / Planning / Action 四个接口 | 职责清晰，无重叠 |
| 2 | 实现至少一个组件 | 实现完整 |
| 3 | 编写组件间调用测试 | 组件可互相调用 |

- [ ] 四个接口定义完毕
- [ ] 组件间调用链路正常

## V2 — Tool Registry

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 注册 3 个不同工具 | 全部注册成功 |
| 2 | 调用 ListTools() | 返回 3 个工具的 JSON Schema |
| 3 | 调用 Execute("get_weather", {"city":"北京"}) | 返回正确结果 |
| 4 | 调用不存在的工具 | 返回错误，不崩溃 |

- [ ] 注册/发现/调用全链路正常
- [ ] 非法调用有错误处理

## V3 — Provider Failover

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 配置两个 Provider，第二个设无效 API Key | — |
| 2 | 发送聊天请求 | 自动切换到可用 Provider |
| 3 | 检查响应 | 正常返回 |

## 验证协议

### V1 — 四大组件
1. 定义 Profile / Memory / Planning / Action 四个接口，各接口职责清晰无重叠
2. 实现至少一个组件，编译通过
3. 编写组件间调用测试，验证调用链路正常

### V2 — Tool Registry
1. 注册 3 个不同工具，调用 `ListTools()` 应返回 3 个工具的 JSON Schema
2. 调用 `Execute("get_weather", {"city":"北京"})` 应返回正确结果
3. 调用不存在的工具应返回 error 而非 panic
4. 用 `go test` 运行，三项全部通过即视为 V2 通过

### V3 — Provider Failover
1. 配置两个 Provider，第二个使用无效 API Key（故意让第二个不可用）
2. 用 mock 让第一个 Provider 返回 error
3. 断言：自动切换到第二个 Provider（即请求仍被处理，不 panic）
4. 切换过程对用户透明（用户看到正常回复）
