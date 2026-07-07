# Mimo CLI 接入 DeepSeek 手册

本文以 DeepSeek 官方 API 为例，说明如何让 `mimo` 命令使用 DeepSeek 或 OpenAI 兼容中转。

## 适用版本

先确认本机命令可用：

```bash
mimo
```

Mimo 支持在 TUI 中添加 Custom Provider，也支持通过配置文件管理。官方 README 中说明配置文件位置为：

```text
项目目录\.mimocode\mimocode.json
%USERPROFILE%\.config\mimocode\mimocode.json
```

也可以使用 providers 命令：

```bash
mimo providers list
mimo models
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

## 推荐配置方式：TUI 添加 Custom Provider

运行：

```bash
mimo
```

在首次配置或设置界面中选择：

```text
Custom Provider
OpenAI-compatible API
```

填写：

```text
Base URL: https://api.deepseek.com
API Key: sk-你的DeepSeekKey
Model: deepseek-v4-pro
```

保存后可以查看模型：

```bash
mimo models --refresh
```

## 命令行选择模型

Mimo 的模型参数格式通常是：

```text
provider/model
```

示例：

```bash
mimo
```

如果你的 provider ID 不是 `deepseek`，请使用 `mimo providers list` 中显示的 ID。

## 验证

```bash
mimo
```

如果能正常进入或返回，VulnPilot 中选择 `mimo` 后即可使用该配置。

## 注意

- Mimo 的 provider ID 以本机 `mimo providers list` 显示为准。
- VulnPilot 只负责启动 `mimo` 命令，不会覆盖你的 Mimo 模型配置。
