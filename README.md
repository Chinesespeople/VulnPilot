# VulnPilot

当前发布：`Release 18`

VulnPilot 是一个 Burp Suite Java 扩展，用于把 Burp 中的 HTTP 流量、HOST 资产、工作空间和本地智能体连接起来，辅助完成渗透测试、代码审计、漏洞证据整理、报告生成、参数加密链还原和 Intruder Payload Processing 编码器生成。

## 下载

- [下载 VulnPilot.jar](https://github.com/Chinesespeople/VulnPilot/releases/latest/download/VulnPilot.jar)
- [下载 SHA256SUMS.txt](https://github.com/Chinesespeople/VulnPilot/releases/latest/download/SHA256SUMS.txt)
- [查看 Release 18](https://github.com/Chinesespeople/VulnPilot/releases/tag/release-18)

仓库中也保留了相同的发布文件：

- [`dist/VulnPilot.jar`](dist/VulnPilot.jar)
- [`SHA256SUMS.txt`](SHA256SUMS.txt)

`VulnPilot.jar` 是已经过混淆处理的发布版本。本次发布的 SHA-256 为：

```text
2abfce0d0a94bef9eff054fce93fe1f9af799803aac03215fc5adadfff388bfa
```

## 主要功能

### HOST 与漏洞管理

- 按 HOST 聚合 Burp 流量、任务进度、请求数量和漏洞结果。
- 支持直接新建 HOST、继续深入渗透测试和删除项目。
- HOST 可绑定源码目录，将流量验证与代码审计结合为白盒测试。
- 漏洞发现会增量写入列表，不需要等待整轮测试结束。
- 漏洞可关联 `CAP-*` 固定快照，保留真实请求、响应、协议、HOST 和端口信息。
- 支持把带有真实请求与响应证据的漏洞写入 Burp 原生漏洞列表。

### 工作空间

- 可创建工作空间分组，并从 Burp 数据包右键菜单复制完整请求、响应和基础信息。
- 工作空间详细窗口可浏览和删除组内数据包，并查看漏洞结果。
- 支持根据组内多个关联数据包进行渗透测试，或结合已有文件、漏洞和报告继续测试。
- 工作空间与 HOST 都可以绑定源码目录，用于白盒代码审计和动态验证。
- 支持分析和还原加密链，结合数据包、站点资源与源码还原整包参数生成流程。
- 生成的脚本需要经过实际运行和验证，分析报告与脚本统一保存在工作目录中。

### 智能体任务

- 当前支持 Claude、Codex、Mimo 和 Qwen，本机存在对应命令时才会显示。
- 默认使用 Claude，智能体的启动方式和输出解析由插件统一管理。
- 支持任务排队、持久化、取消、会话恢复和用户补充提示词。
- 任务输出区展示中文对话、工具调用和等待授权状态。
- 风险操作授权可分别配置渗透测试动作和本地工具动作，并支持用户补充意见。
- 支持多个智能体任务并行运行，每个授权请求与对应任务独立绑定。

### 参数加密链与编码器

- 可从 Burp 数据包右键发起参数加密链分析。
- 支持结合请求、响应和站点资源分析参数的生成、签名、编码与加密逻辑。
- 可按指定语言生成经过验证的还原脚本和报告。
- 可生成 Intruder Payload Processing Java 编码器，并在编码器列表中测试和管理。

### MCP 服务

- 插件内置基于 HTTP 的 MCP 服务，默认监听 `127.0.0.1:19960`。
- MCP 访问必须使用配置页生成的 Token 鉴权。
- 可读取 Burp 当前内存中的 Proxy HTTP history 与 Site Map，不会为了读取缓存而重新请求目标。
- 支持正则检索 HTTP 历史、按路由聚合 Site Map，并精确读取匹配的数据包。
- 支持读取 HOST、源码绑定、工作空间数据包、漏洞摘要和模型请求历史。
- 支持通过 Burp 发送完整 RAW HTTP 请求，并把请求与响应保存为 `CAP-*` 固定快照。
- 大型请求或响应可分片读取，也可直接在完整快照中搜索关键词。
- 回写版 MCP 可把漏洞、对攻击有利的信息和报告增量同步到对应 HOST 或工作空间。
- 可配置是否允许回写漏洞、报告、Burp 原生漏洞以及是否向客户端暴露源码目录。

### 模型请求历史

- 单独保存 MCP 主动请求和用户手动加入的数据包快照，与 Burp Proxy HTTP history 相互独立。
- 每条记录显示快照 ID、请求信息和响应信息，可在内置请求/响应编辑器中查看。
- 最大保存数量可在配置中设置，也可以选择不限制。

## 界面

加载扩展后，Burp Suite 会出现 `VulnPilot` 顶层选项卡，主要页面包括：

- `HOST列表`
- `工作空间`
- `任务`
- `编码器`
- `配置`
- `智能体任务`
- `模型请求历史`

智能体代理、风险授权、MCP 和默认请求头等设置位于 `配置` 页面。

## 安装到 Burp Suite

1. 下载最新的 `VulnPilot.jar`。
2. 打开 Burp Suite，进入 `Extensions` -> `Installed`。
3. 点击 `Add`，Extension type 选择 `Java`。
4. 选择下载的 `VulnPilot.jar`。
5. 加载成功后打开 `VulnPilot` 顶层选项卡。

升级时请先在 Burp Suite 中移除旧版本扩展，再加载新 JAR。

## 配置智能体

进入 `VulnPilot` -> `配置` -> `智能体代理`。插件启动时会检测本机可用的内置智能体命令，只显示能够启动的智能体。

当前支持：

- `claude`
- `codex`
- `mimo`
- `qwen`

可以先在系统命令行分别运行命令名确认是否安装成功：

```text
claude
codex
mimo
qwen
```

### 通过 Node.js 安装

先安装 [Node.js LTS](https://nodejs.org/)，然后按需安装智能体 CLI：

```bash
npm install -g @anthropic-ai/claude-code
npm install -g @openai/codex
npm install -g @mimo-ai/cli
npm install -g @qwen-code/qwen-code
```

首次运行对应命令时，按照该 CLI 的提示完成登录或模型配置。

## 连接 MCP

进入 `VulnPilot` -> `配置` -> `MCP 服务`，确认服务状态正常，然后复制界面显示的地址和鉴权信息。

默认提供两个地址：

- 回写版：`http://127.0.0.1:19960/mcp`
- 基础版：`http://127.0.0.1:19960/mcp/basic`

回写版会根据配置暴露漏洞与报告同步工具；基础版不暴露这些回写工具。实际地址、端口和 Token 以插件配置页显示的内容为准。

常见 HTTP MCP 客户端可参考以下结构：

```json
{
  "mcpServers": {
    "vulnpilot-burp": {
      "type": "http",
      "url": "http://127.0.0.1:19960/mcp",
      "headers": {
        "Authorization": "Bearer <VulnPilot 配置页显示的 Token>"
      }
    }
  }
}
```

不同客户端的 MCP 配置字段可能不同，连接信息应使用配置页的复制按钮获取。

## 智能体模型修改手册

需要调整智能体使用的模型或 OpenAI 兼容服务时，可参考：

- [Claude Code 接入 DeepSeek](docs/agents/claude-deepseek.md)
- [Codex CLI 接入 DeepSeek](docs/agents/codex-deepseek.md)
- [Mimo CLI 接入 DeepSeek](docs/agents/mimo-deepseek.md)
- [Qwen Code 接入 DeepSeek](docs/agents/qwen-deepseek.md)

## 数据目录

配置数据保存在当前用户的软件配置目录下：

```text
Windows: %APPDATA%\burpsuite-vulnpilot
macOS: ~/Library/Application Support/burpsuite-vulnpilot
Linux: $XDG_CONFIG_HOME/burpsuite-vulnpilot 或 ~/.config/burpsuite-vulnpilot
```

智能体工作目录默认位于 Java 临时目录中的 `burpsuite-vulnpilot`，可以在配置中修改。任务、报告、漏洞 JSON、工作空间文件和证据快照会保存在对应工作目录中。

## 校验文件

Linux 或 macOS 下载 Release 附件后：

```bash
sha256sum VulnPilot.jar
```

Windows PowerShell 下载 Release 附件后：

```powershell
Get-FileHash -Algorithm SHA256 .\VulnPilot.jar
```

输出应为：

```text
2ABFCE0D0A94BEF9EFF054FCE93FE1F9AF799803AAC03215FC5ADADFFF388BFA
```

## 使用范围

VulnPilot 仅用于获得明确授权的安全测试、开发验证和教学环境。涉及文件上传、命令执行、数据修改等风险操作时，应根据测试范围配置授权规则并核对智能体实际操作。
