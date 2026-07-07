# VulnPilot

当前版本：`1.0.0.0`

VulnPilot 是一个 Burp Suite 扩展，用于把 Burp 流量与本地智能体工具联动起来，辅助进行站点渗透测试、漏洞证据整理、报告生成、参数加密链分析和 Intruder Payload Processing 编码器生成。

## 下载文件

- 扩展文件：`dist/VulnPilot.jar`
- 校验文件：`SHA256SUMS.txt`

## 主要能力

- 在 Burp Suite 中显示 HOST 列表，并按 HOST 汇总漏洞结果。
- 支持右键把数据包交给默认智能体进行渗透测试。
- 支持对 HOST 继续深入测试，并把多次结果合并到同一个 HOST 项目。
- 支持保存请求/响应证据文件，漏洞详情界面可查看对应 HTTP 包。
- 支持 Claude、Codex、Mimo、Qwen 等本地智能体，启动时会自动检测本机可用命令。
- 支持智能体任务管理、任务数量限制、输出查看、定时巡检历史流量。
- 支持参数加密链分析，并可生成用于 Intruder Payload Processing 的编码器。
- 支持生成中文漏洞描述、利用链、影响说明和修复建议。

## 安装到 Burp Suite

1. 下载 `dist/VulnPilot.jar`。
2. 打开 Burp Suite。
3. 进入 `Extensions` -> `Installed`。
4. 点击 `Add`。
5. Extension type 选择 `Java`。
6. 选择下载的 `VulnPilot.jar`。
7. 加载成功后会出现 `VulnPilot` 顶层选项卡。

## 配置智能体

进入 `VulnPilot` -> `智能体代理`，插件会在启动时自动检测本机可用的内置智能体命令，仅显示可用项。

当前支持：

- `claude`
- `codex`
- `mimo`
- `qwen`

默认使用 Claude。命令参数由插件内置管理，界面只保留必要的工作目录、系统提示词、报告生成和风险授权配置。

建议先在系统命令行确认对应智能体命令可以正常运行，例如：

```bash
claude
codex
mimo
qwen
```

如果某个命令不可用，该智能体不会出现在插件智能体列表中。

## 智能体模型修改手册

如果需要把智能体切换到 DeepSeek 或 OpenAI 兼容中转，可参考：

- [Claude Code 接入 DeepSeek](docs/agents/claude-deepseek.md)
- [Codex CLI 接入 DeepSeek](docs/agents/codex-deepseek.md)
- [Mimo CLI 接入 DeepSeek](docs/agents/mimo-deepseek.md)
- [Qwen Code 接入 DeepSeek](docs/agents/qwen-deepseek.md)

## 通过 Node.js 安装智能体命令

先安装 Node.js，建议使用官方 LTS 版本：

[https://nodejs.org/](https://nodejs.org/)

安装完成后确认命令可用：

```bash
node -v
npm -v
```

按需安装智能体 CLI：

```bash
npm install -g @anthropic-ai/claude-code
npm install -g @openai/codex
npm install -g @mimo-ai/cli
npm install -g @qwen-code/qwen-code
```

确认安装成功：

```bash
claude
codex
mimo
qwen
```

首次使用时按对应 CLI 的提示完成登录或认证配置。VulnPilot 会在启动时自动检测这些命令，未安装的智能体不会显示在插件列表中。

## 数据目录

VulnPilot 会把配置、任务、报告、证据文件保存到用户本机软件数据目录下的：

```text
burpsuite-vulnpilot
```

## 使用建议

- 仅在授权测试环境中使用。
- 进行长时间智能体任务前，建议先配置任务并发数量。
- 对重要 HOST 可使用继续深入渗透测试，让智能体基于已有结果补充利用链和证据。
- 若发现漏洞详情中的请求/响应为空，优先检查对应 HOST 项目目录里的 `.captures` 证据文件是否存在。

## 校验 JAR

下载后可使用 SHA256 校验文件完整性：

```bash
sha256sum -c SHA256SUMS.txt
```

Windows PowerShell 可使用：

```powershell
Get-FileHash -Algorithm SHA256 .\dist\VulnPilot.jar
```
