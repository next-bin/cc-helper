# cc-helper

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE)
[![Node](https://img.shields.io/badge/Node-%3E%3D14.0.0-green.svg)](https://nodejs.org)

[**English**](./README.md) | **简体中文**

> ⚡ 一键启用 Claude Code CLI 的隐藏功能：`/loop` 和 `/btw`

## 环境要求

- Node.js >= 14.0.0
- Claude Code v2.1.71+

```bash
npm install -g @anthropic-ai/claude-code@v2.1.71
```

## 使用方法

```bash
# 启用所有功能（默认）
npx @unitsvc/cc-helper enable

# 查看状态
npx @unitsvc/cc-helper status

# 禁用功能（恢复原状）
npx @unitsvc/cc-helper disable
```

### 代理支持

如果下载失败，使用 `--proxy` 参数：

```bash
# 使用默认代理 (https://edgeone.gh-proxy.org)
npx @unitsvc/cc-helper --proxy enable

# 使用自定义代理
npx @unitsvc/cc-helper --proxy https://your-proxy.com enable
```

### 命令说明

| 命令      | 说明                    |
| --------- | ----------------------- |
| `enable`  | 启用 `/loop` 和 `/btw` 功能 |
| `disable` | 恢复原始状态            |
| `status`  | 查看当前状态            |

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

## 功能特点

- 一键启用 `/loop` 和 `/btw` 功能
- 轻松恢复原始状态
- 自动备份原文件
- 零运行时依赖
- 跨平台支持

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
