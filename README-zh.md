# cc-helper

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg)](https://www.npmjs.com/package/@unitsvc/cc-helper) [![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE) [![Node](https://img.shields.io/badge/Node-%3E%3D14.0.0-green.svg)](https://nodejs.org) [![CLAUDE](https://img.shields.io/badge/Claude%20Code-v2.1.71%2B-green.svg)](https://docs.anthropic.com/en/docs/claude-code)

[**English**](./README.md) | [**简体中文**](./README-zh.md)

> 一键解锁 Claude Code 隐藏超能力：`/loop`、`/btw`、`/keybindings`、`/context1m` 和 `MCPSearch`

## 目录

| 分类         | 内容                                                                                                                        |
| ------------ | --------------------------------------------------------------------------------------------------------------------------- |
| **快速开始** | [环境要求](#环境要求) · [安装](#安装) · [使用方法](#使用方法) · [命令说明](#命令说明)                                       |
| **核心功能** | [`/loop`](#什么是-loop) · [`/btw`](#什么是-btw) · [`/keybindings`](#什么是-keybindings) · [`/context1m`](#什么是-context1m) |
| **工具搜索** | [概述](#什么是工具搜索) · [配置说明](#配置说明)                                                                             |
| **配置命令** | [plan](#plan-命令) · [vault](#vault-命令) · [env](#env-命令) · [sync](#sync-命令)                                           |
| **更多**     | [功能特点](#功能特点) · [支持平台](#支持平台) · [许可证](#许可证) · [安全](#安全)                                           |

---

## 环境要求

| 项目        | 版本      |
| ----------- | --------- |
| Node.js     | >= 14.0.0 |
| Claude Code | v2.1.71+  |

```bash
npm install -g @anthropic-ai/claude-code@v2.1.76
```

## 安装

两种使用方式（二选一）：

```bash
# 全局安装（可选）
npm install -g @unitsvc/cc-helper@latest

# 或直接运行，无需安装
npx @unitsvc/cc-helper@latest enable
```

### 代理支持

如果下载失败，使用 `--proxy` 参数：

```bash
# 使用默认代理
npx @unitsvc/cc-helper --proxy enable

# 使用自定义代理
npx @unitsvc/cc-helper --proxy https://your-proxy.com enable
```

## 使用方法

```bash
# 启用默认功能（/loop, /btw, /keybindings）
npx @unitsvc/cc-helper enable

# 启用特定功能
npx @unitsvc/cc-helper enable loop
npx @unitsvc/cc-helper enable btw
npx @unitsvc/cc-helper enable keybindings
npx @unitsvc/cc-helper enable toolsearch
npx @unitsvc/cc-helper enable context1m   # 别名: 1m, 1M

# 查看状态
npx @unitsvc/cc-helper status

# 禁用所有功能
npx @unitsvc/cc-helper disable
```

## 命令说明

| 命令                 | 说明                                         |
| -------------------- | -------------------------------------------- |
| `enable`             | 启用 `/loop`、`/btw`、`/keybindings`（默认） |
| `enable loop`        | 仅启用 `/loop`                               |
| `enable btw`         | 仅启用 `/btw`                                |
| `enable keybindings` | 仅启用 `/keybindings`                        |
| `enable toolsearch`  | 启用 toolsearch（需要显式激活）              |
| `enable context1m`   | 启用 1M 上下文（v2.1.76+）                   |
| `disable`            | 恢复原始状态                                 |
| `status`             | 查看当前状态及版本要求                       |

> **注意**: 运行 `cc-helper enable` 时会自动在 `~/.claude/settings.json` 中配置推荐的环境变量：
>
> ```json
> {
>   "env": {
>     "DISABLE_INSTALLATION_CHECKS": "1",
>     "DISABLE_AUTOUPDATER": "1",
>     "DISABLE_BUG_COMMAND": "1",
>     "DISABLE_ERROR_REPORTING": "1",
>     "DISABLE_TELEMETRY": "1",
>     "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
>     "CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY": "1",
>     "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
>     "CLAUDE_CODE_HIDE_ACCOUNT_INFO": "1",
>     "API_TIMEOUT_MS": "3000000"
>   }
> }
> ```

---

## 什么是 `/loop`？

定时重复提示，适用于轮询部署、监控 PR、设置提醒、定时执行工作流。

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

| 形式       | 示例                        | 解析间隔     |
| ---------- | --------------------------- | ------------ |
| 前置时间   | `/loop 30m check`           | 每 30 分钟   |
| 后置 every | `/loop check every 2 hours` | 每 2 小时    |
| 无间隔     | `/loop check`               | 默认 10 分钟 |

支持单位：`s`（秒）、`m`（分）、`h`（时）、`d`（天）

### 核心特性

- **会话级别**：任务仅在当前会话中存在，退出即消失
- **自动过期**：3 天后自动过期
- **抖动保护**：小偏移量防止 API 惊群效应
- **低优先级**：在你与 Claude 交互间隙触发

### 管理任务

```
what scheduled tasks do I have?   # 列出所有任务
cancel the deploy check job       # 按描述或 ID 取消
```

## 什么是 `/btw`？

在不打断主对话的情况下提问旁支问题。

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

## 什么是 `/keybindings`？

自定义键盘快捷键。在 `~/.claude/keybindings.json` 中配置：

```json
{
  "submit": ["ctrl+s"],
  "interrupt": ["ctrl+c"],
  "custom_commands": {
    "ctrl+shift+l": "/loop 5m check status"
  }
}
```

## 什么是 `/context1m`？

为 Claude Opus 模型启用 1M token 上下文窗口。

### 要求

- Claude Code v2.1.76 或更高版本
- Claude Opus 4+ 模型
- 可能需要 Pro 计划或官方 API

### 使用方法

```bash
npx @unitsvc/cc-helper enable context1m  # 别名: 1m, 1M
```

### 扩展思维与上下文长度

| 模型                 | 最大思维链长度 | 上下文长度 |
| -------------------- | -------------- | ---------- |
| qwen3.5-plus         | 81,920         | 1,000,000  |
| qwen3-coder-plus     | 不支持         | 1,000,000  |
| qwen3-max-2026-01-23 | 81,920         | 262,144    |
| qwen3-coder-next     | 不支持         | 262,144    |
| kimi-k2.5            | 81,920         | 262,144    |
| MiniMax-M2.5         | 32,768         | 204,800    |
| glm-5                | 32,768         | 202,752    |
| glm-4.7              | 32,768         | 202,752    |

---

## 什么是工具搜索？

在运行时动态搜索和加载工具，而非一次性发送所有工具定义。节省 token 并提高性能。

### 为什么为第三方 API 启用？

Claude Code 在使用第三方 API 代理时默认禁用工具搜索。此功能为这些代理启用工具搜索。

### 优势

- **Token 效率**：减少大型 MCP 工具目录的上下文占用
- **性能提升**：延迟加载，响应更快
- **代理兼容**：支持 Kimi 和其他提供商

### 要求

- 代理必须支持 API 响应中的 `tool_reference` 块
- 仅支持 Claude Sonnet 4+ 和 Opus 4+ 模型（不支持 Haiku）

### 配置说明

通过 `ENABLE_TOOL_SEARCH` 环境变量控制：

| 值         | 行为                              |
| ---------- | --------------------------------- |
| （未设置） | 默认启用，非第一方主机时禁用      |
| `true`     | 始终启用                          |
| `auto`     | MCP 工具超过上下文 10% 时激活     |
| `auto:<N>` | 自定义阈值（如 `auto:5` 表示 5%） |
| `false`    | 禁用，所有工具预先加载            |

```bash
ENABLE_TOOL_SEARCH=auto:5 claude   # 5% 阈值
ENABLE_TOOL_SEARCH=false claude    # 禁用
ENABLE_TOOL_SEARCH=true claude     # 始终启用
```

#### 禁用 MCPSearch 工具

```json
{
  "permissions": {
    "deny": ["MCPSearch"]
  }
}
```

---

## plan 命令

配置 AI Provider，支持 vault 加密存储。

```bash
# 添加 Provider（自动保存到 vault + settings.json）
cc-helper plan add -p bailian -k YOUR_API_KEY
cc-helper plan add -p minimaxi -k YOUR_API_KEY --mcp

# 切换 Provider
cc-helper plan switch -p zai

# 列出 Provider
cc-helper plan list

# 导出配置
cc-helper plan export --all-env -o config.json
```

**支持的 Provider：**

| Provider   | 说明         |
| ---------- | ------------ |
| `bailian`  | (CN) Aliyun  |
| `minimaxi` | (CN) MiniMax |
| `glm`      | (CN) Zhipu   |
| `zai`      | (EN) Zhipu   |

## vault 命令

安全的密钥存储，加密存储在 `cc-helper.json` 中。

```bash
cc-helper vault list                    # 列出密钥
cc-helper vault set bailian default -k "KEY"   # 设置
cc-helper vault get bailian default              # 获取并解密
cc-helper vault delete bailian default          # 删除
```

## env 命令

多环境配置（default、work、staging 等）。

```bash
cc-helper env list    # 列出环境
cc-helper env create work   # 创建
cc-helper env switch work   # 切换
```

## sync 命令

将配置导出/导入到 Git 仓库，支持 JWE 加密。

```bash
# 登录 GitHub
cc-helper sync login -r https://github.com/user/repo -t ghp_xxx

# 导出
cc-helper sync export
cc-helper sync export --file config.json
cc-helper sync export --workspace test

# 导入
cc-helper sync import
```

---

## 功能特点

| 功能          | 说明                                         |
| ------------- | -------------------------------------------- |
| 一键启用      | 启用 `/loop`、`/btw`、`/keybindings`         |
| 工具搜索      | 可选 `/toolsearch` 用于第三方 API 代理       |
| 1M 上下文     | 可选 `/context1m` 用于 1M 上下文（v2.1.76+） |
| Provider 配置 | `plan` 命令支持 vault 加密存储               |
| 密钥管理      | `vault` 命令安全管理密钥                     |
| 多环境支持    | `env` 命令环境切换                           |
| Git 同步      | `sync` 命令配置同步                          |
| 轻松恢复      | 自动备份和恢复                               |
| 零依赖        | 无运行时依赖                                 |

### 截图

![/loop 命令提示](./docs/images/loop-1.png)
![/loop 执行示例](./docs/images/loop-2.png)

## 支持平台

| 平台    | 架构         |
| ------- | ------------ |
| macOS   | amd64, arm64 |
| Linux   | amd64, arm64 |
| Windows | amd64, arm64 |

## 许可证

AGPL-3.0 - 详见 [LICENSE](./LICENSE)

## 安全

### 报告漏洞

如果您发现 cc-helper 的安全漏洞，请负责任地报告：

1. **不要**公开提 issue
2. 发送邮件给维护者说明详情
3. 给予合理时间修复后再公开

我们非常重视安全问题，会尽快响应。
