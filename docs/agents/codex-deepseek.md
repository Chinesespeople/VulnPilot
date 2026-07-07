# Codex CLI 接入 DeepSeek 手册

本文以 DeepSeek 官方 API 为例，说明如何让 `codex` 命令使用 DeepSeek 或 OpenAI 兼容中转。

## 适用版本

先确认本机命令可用：

```bash
codex --version
codex --help
```

Codex CLI 读取用户配置：

```text
%USERPROFILE%\.codex\config.toml
```

也可以用命令行 `-c key=value` 临时覆盖配置。不同 Codex 版本支持的 provider 字段可能变化，修改前建议先看：

```bash
codex --help
codex exec --help
codex debug models
```

## DeepSeek 模型

截至 2026-07-07，DeepSeek 官方 OpenAI 兼容入口为：

```text
https://api.deepseek.com
```

新版模型名：

```text
deepseek-v4-flash
deepseek-v4-pro
```

旧模型名 `deepseek-chat`、`deepseek-reasoner` 已标注将于 2026-07-24 弃用。

## 配置示例

编辑：

```text
%USERPROFILE%\.codex\config.toml
```

添加或调整：

```toml
model_provider = "deepseek"
model = "deepseek-v4-pro"

[model_providers.deepseek]
name = "deepseek"
base_url = "https://api.deepseek.com"
wire_api = "chat"
requires_openai_auth = true
```

然后设置环境变量：

```powershell
$env:OPENAI_API_KEY="sk-你的DeepSeekKey"
```

如果你的 Codex 版本要求 Responses API，可以把 `wire_api` 改为该版本帮助信息或现有配置中支持的值。若不确定，优先保留你当前 `config.toml` 中已经能工作的 `wire_api` 字段，只替换 `base_url`、`model` 和 Key。

## 临时命令行覆盖

```powershell
$env:OPENAI_API_KEY="sk-你的DeepSeekKey"
codex exec --json --dangerously-bypass-approvals-and-sandbox --skip-git-repo-check `
  -c model_provider='"deepseek"' `
  -c model='"deepseek-v4-pro"' `
  "请只回复：CODEX_DEEPSEEK_OK"
```

## 验证

```bash
codex exec --json --dangerously-bypass-approvals-and-sandbox --skip-git-repo-check "请只回复：CODEX_DEEPSEEK_OK"
```

如果能看到正常 JSON 输出，VulnPilot 中选择 `codex` 后即可使用该配置。

## 注意

- 不要把真实 API Key 写入公开仓库。
- Codex 的第三方 provider 字段随版本变化较快，以当前 `codex --help` 和本机 `~/.codex/config.toml` 为准。
- VulnPilot 只负责启动 `codex` 命令，不负责替用户保存 Codex 的模型配置。
