# Claude Code 接入 DeepSeek 手册

本文以 DeepSeek 官方 API 为例，说明如何让 `claude` 命令使用 DeepSeek 模型。

## 适用版本

先确认本机命令可用：

```bash
claude
```

Claude Code 支持通过环境变量配置 Anthropic 协议兼容服务。DeepSeek 官方 Anthropic 兼容入口为：

```text
https://api.deepseek.com/anthropic
```

截至 2026-07-07，DeepSeek 官方文档中的新版模型名为：

```text
deepseek-v4-flash
deepseek-v4-pro
```

旧模型名 `deepseek-chat`、`deepseek-reasoner` 已标注将于 2026-07-24 弃用，建议新配置使用新版模型名。

## Windows PowerShell 临时配置

```powershell
$env:ANTHROPIC_AUTH_TOKEN="sk-你的DeepSeekKey"
$env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
$env:ANTHROPIC_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
claude
```

## 写入 Claude Code 配置

也可以把环境变量写入用户配置文件：

```text
%USERPROFILE%\.claude\settings.json
```

示例：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-你的DeepSeekKey",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash"
  }
}
```

## 验证

```bash
claude
```

如果能正常启动并完成认证，VulnPilot 中选择 `claude` 后即可使用该配置。

## 注意

- 如果使用中转服务，把 `ANTHROPIC_BASE_URL` 改成中转提供的 Anthropic 兼容地址。
- VulnPilot 会启动系统中的 `claude` 命令，不单独覆盖你的模型配置。
