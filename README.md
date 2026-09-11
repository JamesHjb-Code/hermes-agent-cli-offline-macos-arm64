# Hermes Agent CLI Offline Install

This package installs Nous Research Hermes Agent CLI on Apple Silicon macOS without network access.

中文用户请优先阅读 DMG 根目录里的 `安装说明书.txt`。

## Online Build Result

- Hermes Agent: `0.21.1`
- Commit: `45a6101f36576367359c171cd5820ee76a3d047b`
- uv: `0.12.13`
- Python: `3.11.15`
- Node: `26.8.2`
- npm: `11.19.1`
- Playwright Chromium: `1234`

The package intentionally does not include `.env`, auth files, API keys, session logs, or `state.db`.

## Offline Install

Open the DMG or unpack the archive on an Apple Silicon Mac.

For non-technical users, double-click these files in order:

1. `1-双击安装Hermes.command`
2. `2-配置API中转站.command`
3. `3-测试Hermes.command`

If macOS blocks a `.command` file, Control-click it, choose Open, then confirm Open again. You can also run the commands manually from Terminal:

```bash
cd "/Volumes/Hermes Offline Installer"
./1-双击安装Hermes.command
./2-配置API中转站.command
./3-测试Hermes.command
```

Manual terminal install:

```bash
cd hermes-offline-macos-arm64
./install-offline.sh
```

Optional custom install location:

```bash
HERMES_HOME="$HOME/.hermes" ./install-offline.sh
```

If `~/.local/bin` is not on PATH, add:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

## Configure API Proxy

For users who do not want to edit files manually, run:

```bash
cd hermes-offline-macos-arm64
./configure-api.sh
```

It will ask for:

- API proxy URL, for example `https://your-proxy.example.com/v1`
- API key, for example `sk-...`
- Model name, for example `gpt-4o-mini`

The script writes `OPENAI_API_KEY` and `OPENAI_BASE_URL` to `~/.hermes/.env`, then sets Hermes to use the `openai-api` provider.

## Verify

```bash
hermes --version
hermes doctor
hermes -z "你好，简单回复一句，说明你可以正常工作。"
```

`hermes doctor` is expected to report that `.env` or auth providers are not configured until you run:

```bash
hermes setup
```

## Contents

- `vendor/official-install.sh`: official installer snapshot.
- `vendor/hermes-agent/`: installed Hermes Agent source tree plus prepared dependencies.
- `vendor/hermes-agent.git.bundle`: source git bundle pinned to the recorded commit.
- `vendor/hermes-bin/`: managed `uv`.
- `vendor/uv-python/`: uv-managed Python 3.11.15 for macOS arm64.
- `vendor/uv-cache/`: Python package cache for offline `uv sync`.
- `vendor/node/`: Node.js 26.8.2 for macOS arm64.
- `vendor/npm-cache/`: npm package cache for offline workspace install.
- `vendor/playwright-browsers/`: Chromium browser binaries for Playwright.

## Uninstall

```bash
rm -rf "$HOME/.hermes"
rm -f "$HOME/.local/bin/hermes" "$HOME/.local/bin/hermes-agent" "$HOME/.local/bin/hermes-acp"
```
