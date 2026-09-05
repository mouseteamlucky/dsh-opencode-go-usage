# dsh-opencode-go-usage

A [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) web plugin: a **floating widget** pinned to the right edge of the page that shows real-time **OpenCode Go** subscription usage — **rolling / weekly / monthly** windows (percent used, progress bar, and reset countdown) for **every API key** in the pool.

> This repository is a maintained, account-scoped release of
> [@xiaweiliang060035/dsh-opencode-go-usage](https://github.com/xiaweiliang060035/dsh-opencode-go-usage)
> (MIT). Install command and branding are updated for `@mouseteamlucky/*`.

## Features

- **Floating widget**: compact button pinned to the right edge; the badge shows the **worst-window** percentage across all keys (green / orange / red breathing pulse by urgency).
- **Expanded panel**: one card per key (active key marked ★) with rolling / weekly / monthly usage bars, percentages, reset countdowns; rate-limited windows flagged ⚠; per-key errors shown inline.
- **Live refresh**: the Host polls the official usage endpoint; the panel auto-refreshes and offers a manual refresh.
- **Key pool auto-discovery**: reads `$DSH_HOME/.credentials.yaml` entries (`OPENCODE_GO_KEY_<name>`); falls back to the active key (`OPENCODE_GO_API_KEY`) when no pool exists.
- **Bilingual UI** & **DSH theme tokens** (light/dark).

## Install

```bash
dsh plugin --profile web add github:mouseteamlucky/dsh-opencode-go-usage
```

Then restart the DSH web app (the launcher/restart-guarded path) and refresh the browser page.

## How it works

- **Host** (Node ESM): discovers the key pool (config → `.credentials.yaml` → env layers), resolves each key through the `credentials` service, and calls the official endpoint:

  ```
  GET https://opencode.ai/zen/go/v1/usage
  Authorization: Bearer <API_KEY>
  ```

  Keys never leave the host — the browser only receives an aggregated snapshot.
- **Client** (browser bundle): registers on the public `shell.overlay` slot and polls the Host route `/plugins/dsh-opencode-go-usage/snapshot`.

## License

MIT. See [LICENSE](LICENSE). Upstream fork attribution: [xiaweiliang060035/dsh-opencode-go-usage](https://github.com/xiaweiliang060035/dsh-opencode-go-usage).
