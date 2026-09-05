# dsh-opencode-go-usage

**简体中文** · [English](README.md)

一个 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) WebUI 插件：在页面**右侧边缘**提供常驻悬浮组件，实时展示 **OpenCode Go** 套餐中 **key 池内每个 key** 的用量——**滚动 / 每周 / 每月**三个窗口的已用百分比、进度条与重置倒计时。

> 本仓库是 [@xiaweiliang060035/dsh-opencode-go-usage](https://github.com/xiaweiliang060035/dsh-opencode-go-usage)
> （MIT）的**账号维护版发布**，安装命令与品牌已改为 `@mouseteamlucky/*`。

## 特性

- **悬浮组件**：紧凑按钮固定在页面右侧边缘，角标实时显示**全部 key 中最差窗口**的百分比；颜色分级（绿 / 橙 / 红色脉冲）让你一眼看出是否有 key 接近额度上限。
- **展开面板**：每个 key 一张卡（当前生效 key 带 ★ 标记），展示滚动 / 每周 / 每月用量（进度条 + 百分比 + 距重置时间）；被限流的窗口标 ⚠。
- **实时刷新**：Host 每 60 秒轮询官方用量端点（可配置），面板自动刷新并提供手动刷新。
- **自动发现 key 池**：读取 `$DSH_HOME/.credentials.yaml` 中的 `OPENCODE_GO_KEY_<名>` 条目——**key 数量与命名不写死**；无池时回退显示当前生效键（`OPENCODE_GO_API_KEY`）。
- **中英双语** + **DSH 主题 token**，明暗主题均适配。

## 安装

```bash
dsh plugin --profile web add github:mouseteamlucky/dsh-opencode-go-usage
```

随后重启 DSH Web 应用（launcher / restart-guarded 路径）并刷新浏览器页面。

## 工作原理

- **Host 半**（Node ESM）：发现 key 池（config → `.credentials.yaml` → 环境变量分层），通过 `credentials` 服务解析每个 key，调用官方用量端点：

  ```
  GET https://opencode.ai/zen/go/v1/usage
  Authorization: Bearer <API_KEY>
  ```

  **密钥始终不出 Host**——浏览器只拿聚合快照。
- **Client 半**（浏览器 bundle）：注册在公开槽位 `shell.overlay`，轮询 Host 的路由 `/plugins/dsh-opencode-go-usage/snapshot`。

## 许可证

MIT，见 [LICENSE](LICENSE)。上游出处：[xiaweiliang060035/dsh-opencode-go-usage](https://github.com/xiaweiliang060035/dsh-opencode-go-usage)。
