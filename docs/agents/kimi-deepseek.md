# Kimi Code CLI 接入 DeepSeek 手册

本文以 DeepSeek 官方 API 为例，说明如何让 `kimi` 命令使用 DeepSeek 或 OpenAI Chat Completions 兼容服务。

## 安装 Kimi

Windows PowerShell：

```powershell
Invoke-RestMethod https://code.kimi.com/install.ps1 | Invoke-Expression
```

Linux 或 macOS：

```bash
curl -LsSf https://code.kimi.com/install.sh | bash
```

已经安装 `uv` 时，也可以运行：

```bash
uv tool install --python 3.13 kimi-cli
```

确认命令可用：

```bash
kimi --version
```

## 配置文件

Kimi Code CLI 默认读取：

```text
~/.kimi/config.toml
```

Windows 中对应 `%USERPROFILE%\.kimi\config.toml`。首次运行 Kimi 后，如果文件不存在，CLI 会自动创建默认配置。

Kimi Code CLI 使用 `openai_legacy` Provider 连接 OpenAI Chat Completions 兼容服务。DeepSeek 官方 OpenAI 兼容入口为：

```text
https://api.deepseek.com
```

截至 2026-07-22，DeepSeek 官方提供的模型名为：

```text
deepseek-v4-flash
deepseek-v4-pro
```

两者的上下文长度均为 1M，并支持思考模式。

## config.toml 示例

```toml
default_model = "deepseek-v4-pro"
default_thinking = true

[providers.deepseek]
type = "openai_legacy"
base_url = "https://api.deepseek.com"
api_key = "sk-你的DeepSeekKey"

[models.deepseek-v4-pro]
provider = "deepseek"
model = "deepseek-v4-pro"
max_context_size = 1000000
capabilities = ["thinking"]

[models.deepseek-v4-flash]
provider = "deepseek"
model = "deepseek-v4-flash"
max_context_size = 1000000
capabilities = ["thinking"]
```

`default_model` 必须对应 `[models.*]` 中已经定义的配置名。需要默认使用 Flash 模型时，将其改为 `deepseek-v4-flash`。

## 验证

运行：

```bash
kimi
```

能够正常启动并完成一次对话后，VulnPilot 中选择 Kimi 即会使用同一份模型配置。

也可以临时确认某个已配置模型：

```bash
kimi --model deepseek-v4-pro
```

## 参考文档

- [Kimi Code CLI 安装说明](https://moonshotai.github.io/kimi-cli/en/guides/getting-started.html)
- [Kimi Code CLI Providers and Models](https://moonshotai.github.io/kimi-cli/en/configuration/providers.html)
- [Kimi Code CLI 配置文件](https://moonshotai.github.io/kimi-cli/en/configuration/config-files.html)
- [DeepSeek Models & Pricing](https://api-docs.deepseek.com/quick_start/pricing)
