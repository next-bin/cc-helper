# cc-helper

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg)](https://www.npmjs.com/package/@unitsvc/cc-helper) [![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE) [![Node](https://img.shields.io/badge/Node-%3E%3D14.0.0-green.svg)](https://nodejs.org) [![CLAUDE](https://img.shields.io/badge/Claude%20Code-v2.1.71%2B-green.svg)](https://docs.anthropic.com/en/docs/claude-code)

[**English**](./README.md) | [**简体中文**](./README-zh.md)

> Claude Code 终极增强工具：一键解锁隐藏功能、管理多 AI 提供商、安全存储密钥、多环境配置同步 —— 全部集成在一个 CLI 工具中。

> **⚠️ 声明：智谱（glm/zai）公司及其关联产品禁止使用本软件。**

---

## 环境要求

| 项目        | 版本      |
| ----------- | --------- |
| Node.js     | >= 14.0.0 |
| Claude Code | v2.1.71+  |

```bash
npm install -g @anthropic-ai/claude-code@v2.1.120
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

---

## 快速开始

```bash
# 启用默认功能（/loop, /btw, /keybindings）
npx @unitsvc/cc-helper enable

# 启用特定功能
npx @unitsvc/cc-helper enable loop        # 定时重复提示
npx @unitsvc/cc-helper enable btw         # 旁支问题
npx @unitsvc/cc-helper enable keybindings # 自定义键盘快捷键
npx @unitsvc/cc-helper enable toolsearch  # 动态工具搜索
npx @unitsvc/cc-helper enable context1m   # 1M 上下文（v2.1.76+）
npx @unitsvc/cc-helper enable automode    # 所有模型的自动模式（v2.1.75+）
npx @unitsvc/cc-helper enable monitor     # 流式事件监控（v2.1.100+）

# 查看状态
npx @unitsvc/cc-helper status

# 禁用所有功能
npx @unitsvc/cc-helper disable
```

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
>     "CLAUDE_CODE_NEW_INIT": "1",
>     "CLAUDE_CODE_ATTRIBUTION_HEADER": "0",
>     "API_TIMEOUT_MS": "3000000"
>   }
> }
> ```

---

## 配置命令

管理 Claude Code 的 AI Provider 设置、API 密钥、多环境配置以及配置同步。

### plan — 编码计划配置

配置编码计划的第三方 AI Provider，支持 vault 加密存储 API 密钥，并可自定义模型配置以适应不同编码场景。

```bash
# 添加 Provider（自动保存到 vault + settings.json）
cc-helper plan add -p bailian -k YOUR_API_KEY
cc-helper plan add -p minimaxi -k YOUR_API_KEY --mcp

# 切换 Provider
cc-helper plan switch -p zai

# 切换模型配置（当前 Provider）
cc-helper plan switch --profile 1m

# 切换 Provider 并指定配置
cc-helper plan switch -p bailian -k YOUR_KEY --profile 1m

# 列出 Provider
cc-helper plan list

# 导出配置
cc-helper plan export --all-env -o config.json
```

**支持的 Provider：**

| Provider   | 说明                  |
| ---------- | --------------------- |
| `bailian`  | (CN) Aliyun           |
| `minimaxi` | (CN) MiniMax          |
| ~~`glm`~~  | ~~(CN) Zhipu~~        |
| ~~`zai`~~  | ~~(EN) Zhipu~~        |
| `ark`      | (CN) Ark (Volcengine) |

**模型配置（Model Profiles）：**

每个 Provider 支持多个模型配置。一个配置定义了所有模型层级的映射：

| 字段      | 说明                                        |
| --------- | ------------------------------------------- |
| Model     | 默认模型 (`ANTHROPIC_MODEL`)                |
| Haiku     | 快速模型 (`ANTHROPIC_DEFAULT_HAIKU_MODEL`)  |
| Sonnet    | 均衡模型 (`ANTHROPIC_DEFAULT_SONNET_MODEL`) |
| Opus      | 强力模型 (`ANTHROPIC_DEFAULT_OPUS_MODEL`)   |
| Reasoning | 扩展思维 (`ANTHROPIC_REASONING_MODEL`)      |

**bailian 配置：**

| Profile | Model        | Haiku        | Sonnet       | Opus         | Reasoning    |
| ------- | ------------ | ------------ | ------------ | ------------ | ------------ |
| default | glm-5        | glm-4.7      | glm-5        | glm-5        | glm-5        |
| 5       | glm-5        | glm-5        | glm-5        | qwen3.6-plus | glm-5        |
| 1m      | glm-5        | glm-4.7      | qwen3.5-plus | qwen3.5-plus | glm-5        |
| 3.6     | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus |
| kimi    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    |
| minimax | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 |

**~~glm / zai 配置~~（禁止使用）**

| Profile | Model        | Haiku       | Sonnet  | Opus    | Reasoning |
| ------- | ------------ | ----------- | ------- | ------- | --------- |
| default | glm-5.1      | glm-4.7     | glm-5.1 | glm-5.1 | glm-5.1   |
| 5       | glm-5        | glm-5-turbo | glm-5   | glm-5   | glm-5     |
| 5.1     | glm-5.1      | glm-4.7     | glm-5.1 | glm-5.1 | glm-5.1   |
| 5v      | glm-5v-turbo | glm-4.7     | glm-5.1 | glm-5.1 | glm-5.1   |

**minimaxi 配置：**

| Profile | Model        | Haiku        | Sonnet       | Opus         | Reasoning    |
| ------- | ------------ | ------------ | ------------ | ------------ | ------------ |
| default | MiniMax-M2.7 | MiniMax-M2.7 | MiniMax-M2.7 | MiniMax-M2.7 | MiniMax-M2.7 |

**ark 配置：**

| Profile  | Model            | Haiku                | Sonnet           | Opus                | Reasoning        |
| -------- | ---------------- | -------------------- | ---------------- | ------------------- | ---------------- |
| default  | kimi-k2.6        | kimi-k2.5            | kimi-k2.6        | kimi-k2.6           | kimi-k2.6        |
| kimi     | kimi-k2.6        | kimi-k2.5            | kimi-k2.6        | kimi-k2.6           | kimi-k2.6        |
| doubao   | doubao-seed-code | doubao-seed-2.0-code | doubao-seed-code | doubao-seed-2.0-pro | doubao-seed-code |
| minimax  | minimax-m2.7     | minimax-m2.7         | minimax-m2.7     | minimax-m2.7        | minimax-m2.7     |
| glm      | glm-5.1          | glm-4.7              | glm-5.1          | glm-5.1             | glm-5.1          |
| deepseek | deepseek-v3.2    | deepseek-v3.2        | deepseek-v3.2    | deepseek-v3.2       | deepseek-v3.2    |
| auto     | glm-5.1          | ark-code-latest      | minimax-m2.7     | glm-5.1             | kimi-k2.6        |

```bash
# 示例：在 bailian 上使用 1M 上下文
cc-helper plan add -p bailian -k YOUR_KEY
cc-helper plan switch --profile 1m

# 示例：配置 Ark 使用 auto 配置
cc-helper plan add -p ark -k YOUR_KEY
cc-helper plan switch --profile auto
```

### vault — 安全密钥存储

使用本地加密方式安全存储和读取 Provider 的 API 密钥。

```bash
cc-helper vault list                    # 列出密钥
cc-helper vault set bailian default -k "KEY"   # 设置
cc-helper vault get bailian default              # 获取并解密
cc-helper vault delete bailian default          # 删除
```

### env — 多环境管理

创建并切换相互隔离的配置环境。

```bash
cc-helper env list    # 列出环境
cc-helper env create work   # 创建
cc-helper env switch work   # 切换
```

### sync — Git 配置同步

将 Claude Code 配置备份和恢复到 Git 仓库，全程加密保护。

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
| 自动模式      | 可选 `automode` 用于所有模型（v2.1.75+）     |
| Monitor       | 可选 `monitor` 用于流式事件监控（v2.1.100+） |
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

---

## 核心功能详解

### `/loop` - 定时重复提示

定时重复提示，适用于轮询部署、监控 PR、设置提醒、定时执行工作流。

```
/loop [间隔时间] <提示内容>
```

**示例：**

```
/loop 5m check if the deployment finished
/loop 30m /review-pr 1234
/loop remind me to push the release at 3pm
```

| 形式       | 示例                        | 解析间隔     |
| ---------- | --------------------------- | ------------ |
| 前置时间   | `/loop 30m check`           | 每 30 分钟   |
| 后置 every | `/loop check every 2 hours` | 每 2 小时    |
| 无间隔     | `/loop check`               | 默认 10 分钟 |

支持单位：`s`（秒）、`m`（分）、`h`（时）、`d`（天）

**核心特性：**

- **会话级别**：任务仅在当前会话中存在，退出即消失
- **自动过期**：3 天后自动过期
- **抖动保护**：小偏移量防止 API 惊群效应
- **低优先级**：在你与 Claude 交互间隙触发

```
what scheduled tasks do I have?   # 列出所有任务
cancel the deploy check job       # 按描述或 ID 取消
```

### `/btw` - 旁支问题

在不打断主对话的情况下提问旁支问题。

```
/btw <问题>
```

**示例：**

```
/btw 这个函数是做什么的？
/btw 解释一下这里的错误处理
/btw 为什么这里要用 async/await？
```

### `/keybindings` - 自定义键盘快捷键

在 `~/.claude/keybindings.json` 中配置：

```json
{
  "submit": ["ctrl+s"],
  "interrupt": ["ctrl+c"],
  "custom_commands": {
    "ctrl+shift+l": "/loop 5m check status"
  }
}
```

### `/context1m` - 1M 上下文

为 Claude Opus 模型启用 1M token 上下文窗口。

**要求：**

- Claude Code v2.1.76 或更高版本
- Claude Opus 4+ 模型
- 可能需要 Pro 计划或官方 API

![/context1m 启用](./docs/images/1m-1.png)

**扩展思维与上下文长度：**

| 模型                 | 最大思维链长度 | 上下文长度 |
| -------------------- | -------------: | ---------: |
| qwen3.6-plus         |         81,920 |  1,000,000 |
| qwen3.5-plus         |         81,920 |  1,000,000 |
| qwen3-coder-plus     |         不支持 |  1,000,000 |
| qwen3-max-2026-01-23 |         81,920 |    262,144 |
| qwen3-coder-next     |         不支持 |    262,144 |
| kimi-k2.5            |         81,920 |    262,144 |
| MiniMax-M2.5         |         32,768 |    204,800 |
| glm-5                |         32,768 |    202,752 |
| glm-4.7              |         32,768 |    202,752 |

### 工具搜索

在运行时动态搜索和加载工具，而非一次性发送所有工具定义。节省 token 并提高性能。

**为什么为第三方 API 启用？** Claude Code 在使用第三方 API 代理时默认禁用工具搜索。此功能为这些代理启用工具搜索。

**优势：**

- **Token 效率**：减少大型 MCP 工具目录的上下文占用
- **性能提升**：延迟加载，响应更快
- **代理兼容**：支持 Kimi 和其他提供商

**要求：**

- 代理必须支持 API 响应中的 `tool_reference` 块
- 仅支持 Claude Sonnet 4+ 和 Opus 4+ 模型（不支持 Haiku）

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

禁用 MCPSearch 工具：

```json
{
  "permissions": {
    "deny": ["MCPSearch"]
  }
}
```

### 自动模式

为所有模型和 API 类型启用自动模式，绕过模型限制。

**为什么启用？** Claude Code 限制自动模式仅适用于特定模型（Opus/Sonnet 4.6）和官方 API。此功能为所有模型和第三方代理启用自动模式。

**优势：**

- **通用访问**：自动模式适用于任意模型
- **代理支持**：兼容 Bedrock、Vertex 和第三方 API
- **无限制**：绕过远程配置控制

**要求：**

- Claude Code v2.1.75 或更高版本

```bash
npx @unitsvc/cc-helper enable automode
```

**环境变量：**

| 变量                            | 说明               |
| ------------------------------- | ------------------ |
| `CC_HELPER_AUTO_MODE_MODEL`     | 自定义分类器模型   |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 未指定时的回退模型 |

### Monitor

启用 Monitor 工具用于流式事件监控。

**优势：**

- **流式监控**：实时监控日志、文件变化、API 事件
- **事件驱动工作流**：事件到达时立即响应
- **持久监控**：在会话期间运行长期监控任务

**要求：**

- Claude Code v2.1.98 或更高版本

```bash
npx @unitsvc/cc-helper enable monitor
```

**示例：**

```bash
# 监控日志文件中的错误
tail -f /var/log/app.log | grep --line-buffered "ERROR"

# 监控文件变化
inotifywait -m --format '%e %f' /watched/dir

# 轮询 GitHub 获取新的 PR 评论
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" --jq '.[].body'
  sleep 30
done
```

## 许可证

AGPL-3.0 - 详见 [LICENSE](./LICENSE)

## 安全

### 报告漏洞

如果您发现 cc-helper 的安全漏洞，请负责任地报告：

1. **不要**公开提 issue
2. 发送邮件给维护者说明详情
3. 给予合理时间修复后再公开

我们非常重视安全问题，会尽快响应。
