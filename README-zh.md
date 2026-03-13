# cc-helper

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE)
[![Node](https://img.shields.io/badge/Node-%3E%3D14.0.0-green.svg)](https://nodejs.org)

[**English**](./README.md) | **简体中文**

> ⚡ 一键启用 Claude Code CLI 的隐藏功能：`/loop`、`/btw` 和 `MCPSearch`

## 环境要求

- Node.js >= 14.0.0
- Claude Code v2.1.71+

```bash
npm install -g @anthropic-ai/claude-code@v2.1.71
```

## 使用方法

```bash
# 启用默认功能（/loop, /btw）
npx @unitsvc/cc-helper enable

# 启用特定功能
npx @unitsvc/cc-helper enable loop
npx @unitsvc/cc-helper enable btw
npx @unitsvc/cc-helper enable toolsearch

# 查看状态
npx @unitsvc/cc-helper status

# 禁用功能（恢复原状）
npx @unitsvc/cc-helper disable
```

### 命令说明

| 命令 | 说明 |
|---------|-------------|
| `enable` | 启用 `/loop` 和 `/btw` 功能（默认，不包含 toolsearch） |
| `enable loop` | 仅启用 `/loop` 功能 |
| `enable btw` | 仅启用 `/btw` 功能 |
| `enable toolsearch` | 启用 toolsearch 功能（需要显式激活） |
| `disable` | 恢复原始状态 |
| `status` | 查看当前状态及版本要求 |

## 赞助

<table>
<tr>
<td width="60" valign="middle">
<img src="https://img.shields.io/badge/🚀-GLM-blue?style=for-the-badge" alt="GLM"/>
</td>
<td valign="middle">
<b>GLM Coding Plan</b><br/>
<sub>Full support for <b>Claude Code</b>, <b>Cline</b>, and <b>20+ top coding tools</b> — starting at just <b>$10/month</b></sub><br/>
<a href="https://z.ai/subscribe?ic=1YVKN4IRCQ"><b>👉 Subscribe now — limited-time deal!</b></a>
</td>
</tr>
</table>

---

## 什么是 `/loop`？

`/loop` 是 Claude Code CLI 的内置命令，用于创建定时重复提示。适用于：

- 轮询部署或构建状态
- 监控 PR 状态
- 设置提醒
- 定时执行工作流

### 使用语法

```
/loop [间隔时间] <提示内容>
```

**示例：**

```
/loop 5m check if the deployment finished
/loop 30m /review-pr 1234
/loop remind me to push the release at 3pm
```

### 间隔时间格式

| 形式       | 示例                        | 解析间隔       |
| ---------- | --------------------------- | -------------- |
| 前置时间   | `/loop 30m check`           | 每 30 分钟     |
| 后置 every | `/loop check every 2 hours` | 每 2 小时      |
| 无间隔     | `/loop check`               | 默认每 10 分钟 |

支持单位：`s`（秒）、`m`（分）、`h`（时）、`d`（天）

### 核心特性

- **会话级别**：任务仅在当前 Claude Code 会话中存在，退出即消失
- **自动过期**：重复任务 3 天后自动过期
- **抖动保护**：添加小偏移量防止 API 惊群效应
- **低优先级**：定时提示在你与 Claude 交互间隙触发

### 管理任务

```
what scheduled tasks do I have?   # 列出所有任务
cancel the deploy check job       # 按描述或 ID 取消
```

## 什么是 `/btw`？

`/btw`（by the way）是一个隐藏命令，用于在不打断主对话流程的情况下提问旁支问题。适合快速澄清疑问。

### 使用方法

```
/btw <问题>
```

**示例：**

```
/btw 这个函数是做什么的？
/btw 解释一下这里的错误处理
/btw 为什么这里要用 async/await？
```

## 什么是工具搜索？

工具搜索（Tool Search）是一项允许 Claude 在运行时动态搜索和加载工具的功能，而不是一次性发送所有工具定义。这可以节省 token 并提高性能。

### 为什么为第三方 API 启用？

默认情况下，Claude Code 在使用第三方 API 代理（如 Kimi、自定义端点）时会禁用工具搜索。此功能修改该行为，为这些代理启用工具搜索。

### 优势

- **Token 效率**：减少大型 MCP 工具目录的上下文窗口占用
- **性能提升**：延迟加载工具，响应更快
- **代理兼容**：支持 Kimi 和其他第三方 Claude API 提供商

### 要求

- 您的代理必须支持 API 响应中的 `tool_reference` 块
- 仅支持 Claude Sonnet 4+ 和 Opus 4+ 模型（不支持 Haiku）

### 配置说明

使用官方 Anthropic API 时，工具搜索默认启用。但当 `ANTHROPIC_BASE_URL` 指向非第一方主机时，工具搜索默认禁用（大多数代理不转发 `tool_reference` 块）。

通过 `ENABLE_TOOL_SEARCH` 环境变量控制工具搜索行为：

| 值         | 行为                                                   |
| ---------- | ------------------------------------------------------ |
| （未设置） | 默认启用。当 `ANTHROPIC_BASE_URL` 为非第一方主机时禁用 |
| `true`     | 始终启用，包括非第一方 `ANTHROPIC_BASE_URL`            |
| `auto`     | 当 MCP 工具超过上下文 10% 时激活                       |
| `auto:<N>` | 自定义阈值激活（如 `auto:5` 表示 5%）                  |
| `false`    | 禁用，所有 MCP 工具预先加载                            |

**示例：**

```bash
# 使用 5% 自定义阈值
ENABLE_TOOL_SEARCH=auto:5 claude

# 完全禁用工具搜索
ENABLE_TOOL_SEARCH=false claude

# 始终启用（适用于支持 tool_reference 的代理）
ENABLE_TOOL_SEARCH=true claude
```

或在 `settings.json` 的 env 字段中设置。

#### 禁用 MCPSearch 工具

您还可以使用 `disallowedTools` 设置专门禁用 `MCPSearch` 工具：

```json
{
  "permissions": {
    "deny": ["MCPSearch"]
  }
}
```

## 功能特点

- 一键启用 `/loop`、`/btw` 功能（toolsearch 需要显式激活）
- 可单独启用特定功能
- 轻松恢复原始状态
- 自动备份原文件
- 零运行时依赖
- 跨平台支持
- 支持版本：2.1.71、2.1.72、2.1.73、2.1.74

### 示例

启用后，在 Claude Code 中使用 `/loop` 命令：

![/loop 命令提示](./docs/images/loop-1.png)

执行 loop 命令示例：

![/loop 执行示例](./docs/images/loop-2.png)

## 支持平台

- macOS (amd64, arm64)
- Linux (amd64, arm64)
- Windows (amd64, arm64)

## 许可证

AGPL-3.0 - 详见 [LICENSE](./LICENSE)

## 安全

### 报告漏洞

如果您发现 cc-helper 的安全漏洞，请负责任地报告：

1. **不要**公开提 issue
2. 发送邮件给维护者说明详情
3. 给予合理时间修复后再公开

我们非常重视安全问题，会尽快响应。
