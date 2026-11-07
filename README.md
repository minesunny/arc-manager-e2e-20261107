# ARC Manager end-to-end tests

此公开仓库用于验证 ARC Manager 管理的仓库专属 Runner 池。工作流仅支持手动触发，不会处理 PR 或 push 事件。

## 已完成的运行

- [池 A：冷缓存](https://github.com/minesunny/arc-manager-e2e-20261107/actions/runs/42359970679)
- [池 B：跨池缓存命中](https://github.com/minesunny/arc-manager-e2e-20261107/actions/runs/42360051441)
- [池 A：重建后缓存命中](https://github.com/minesunny/arc-manager-e2e-20261107/actions/runs/42360392183)

三个运行均为 success，检查 Runner 身份、保留 PVC 上的 Node/V8 编译缓存，以及带认证的 HTTP 构建缓存读写；未认证请求必须返回 401。测试通过结果为 42。

## 手动运行

在 ARC Manager 创建绑定本仓库的池，同时关联 Artifact 与 Gradle HTTP 缓存。在 Actions 中选择工作流并填写：

- `pool`：池的 runs-on 名称。
- `nonce`：非敏感的测试标识。
- `expect_cache`：首次选 `cold`，复用选 `hit`，不限制选 `any`。

不要把凭据填入工作流输入。测试结束后的 Runner 池和 Manager 已删除，保留缓存数据；再次运行前需配置可用的池。

此测试不代表 APT/Registry 代理命中、真实 Gradle 构建、DinD 或多个独立集群已完成验收。
