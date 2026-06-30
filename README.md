# VulnPilot

VulnPilot 是一个 Burp Suite 扩展，用于把 Burp 流量与本地智能体工具联动起来，辅助进行站点渗透测试、漏洞证据整理、报告生成和参数加密链分析。

> 本仓库只发布可直接导入 Burp Suite 的 JAR 文件，不包含源码。

## 下载文件

- 扩展文件：`dist/VulnPilot.jar`
- 校验文件：`SHA256SUMS.txt`

## 主要能力

- 在 Burp Suite 中显示 HOST 列表，并按 HOST 聚合漏洞结果。
- 支持右键把数据包交给配置的智能体进行渗透测试。
- 支持对 HOST 继续深入测试，并把多次结果合并到同一个 HOST 项目。
- 支持保存请求/响应证据文件，漏洞详情界面可查看对应 HTTP 包。
- 支持智能体任务管理、任务数量限制、定时巡检历史流量。
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

进入 `VulnPilot` -> `智能体代理`，默认内置 Claude。推荐先确认本机命令行可以正常运行 Claude Code。

默认 Claude 参数建议为：

```bash
-p --bare --dangerously-skip-permissions --system-prompt "$${system_prompt}$$" "$${context}$$"
```

说明：

- `$${system_prompt}$$` 会在运行时替换为系统提示词。
- `$${context}$$` 会在运行时替换为目标 URL、HTTP 数据包或任务上下文。
- 插件会按多平台方式启动进程，不固定依赖 Windows `cmd`。

## 通过 Node.js 安装 Claude Code

先安装 Node.js。建议使用 Node.js 官方 LTS 版本：

[https://nodejs.org/](https://nodejs.org/)

安装完成后，确认命令可用：

```bash
node -v
npm -v
```

通过 npm 安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

确认安装成功：

```bash
claude --version
```

首次使用时按 Claude Code 的提示完成登录或认证配置。

## 数据目录

VulnPilot 会把配置、任务、报告、证据文件保存到用户本机软件数据目录下的：

```text
burpsuite-vulnpilot
```

常见位置示例：

- Windows：`C:\Users\<用户名>\AppData\Local\Temp\burpsuite-vulnpilot`
- macOS/Linux：根据 Java 临时目录和用户目录环境确定

## 使用建议

- 在授权测试环境中使用。
- 进行长时间智能体任务前，建议先配置任务并发数量。
- 对重要 HOST 建议使用“继续深入渗透测试”，让智能体基于已有报告补充利用链和证据。
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
