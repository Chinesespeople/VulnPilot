# Qwen Code 接入 DeepSeek 手册

本文以 DeepSeek 官方 API 为例，说明如何让 `qwen` 命令使用 DeepSeek 或 OpenAI 兼容中转。

## 适用版本

先确认本机命令可用：

```bash
qwen --version
qwen --help
```

Qwen Code 支持 OpenAI、Anthropic、Gemini、Qwen 和第三方 Provider。配置文件通常位于：

```text
%USERPROFILE%\.qwen\settings.json
```

官方 README 建议使用交互式 `/auth` 完成配置，也可以直接编辑 `settings.json`。

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

## 交互式配置

运行：

```bash
qwen
```

在交互界面中执行：

```text
/auth
```

选择 OpenAI 或自定义 OpenAI-compatible Provider，然后填写 DeepSeek 的 Base URL、API Key 和模型。

## settings.json 示例

编辑：

```text
%USERPROFILE%\.qwen\settings.json
```

示例：

```json
{
  "env": {
    "QWEN_DEEPSEEK_API_KEY": "sk-你的DeepSeekKey"
  },
  "modelProviders": {
    "openai": [
      {
        "id": "deepseek-v4-pro",
        "name": "deepseek-v4-pro",
        "baseUrl": "https://api.deepseek.com",
        "envKey": "QWEN_DEEPSEEK_API_KEY",
        "generationConfig": {
          "extra_body": {
            "enable_thinking": true
          }
        }
      }
    ]
  },
  "security": {
    "auth": {
      "selectedType": "openai"
    }
  },
  "model": {
    "name": "deepseek-v4-pro"
  },
  "$version": 4
}
```

## 验证

```bash
qwen -p --output-format stream-json --yolo "请只回复：QWEN_DEEPSEEK_OK"
```

如果能看到 `QWEN_DEEPSEEK_OK`，VulnPilot 中选择 `qwen` 后即可使用该配置。

## 注意

- 不要把真实 API Key 提交到 GitHub。
- 如果使用中转服务，把 `baseUrl` 改成中转提供的 OpenAI 兼容地址。
- `generationConfig.extra_body.enable_thinking` 是否可用取决于模型和服务端，报错时可以删除该段。
- VulnPilot 只负责启动 `qwen` 命令，不会覆盖你的 Qwen 模型配置。
