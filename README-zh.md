<div align="center">

# CC-Helper

### Claude Code、CodeBuddy 与 Codex 的全能助手与 Provider 管理工具

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg?color=blue&label=version)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![npm downloads](https://img.shields.io/npm/dt/@unitsvc/cc-helper.svg?color=green&label=downloads)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows-orange.svg)](https://github.com/next-bin/cc-helper)
[![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE)

[English](README.md) | 简体中文

</div>

多 Provider 管理、MCP 服务器、通道集成和 AI Provider 代理工具包，适用于 Claude Code、CodeBuddy 和 Codex CLI。

> [!TIP]
> **核心特性**
>
> - 一键配置 `cc-helper claude setup`
> - 多 Provider 支持，vault 加密存储 API 密钥
> - 内置 MCP 服务器（微信、MiniMax、小米 TTS）
> - AI Provider 代理，支持请求日志和分析
> - 环境隔离和 Git 配置同步
> - 零运行时依赖

---

- [前置条件](#前置条件)
- [安装](#安装)
- [快速开始](#快速开始)
- [通道与 MCP](#通道与-mcp)
- [Agentic](#agentic)
- [知识库](#知识库)
- [常用命令](#常用命令)
- [文档](#文档)
- [许可证](#许可证)
- [安全](#安全)
- [声明](#声明)

## 前置条件

cc-helper 用于配置和管理以下编码 Agent CLI，按需安装：

```bash
# Claude Code（建议固定版本）
npm install -g @anthropic-ai/claude-code@v2.1.112

# Codex CLI
npm install -g @openai/codex@latest

# CodeBuddy
npm install -g @tencent-ai/codebuddy-code@latest
```

> [!TIP]
> 如需加速安装，可在命令后追加 `--registry=https://registry.npmmirror.com`。

## 安装

```bash
npm install -g @unitsvc/cc-helper@latest
```

> [!NOTE]
> 需要 Node.js >= 14.0.0。请至少安装一个受支持的 CLI：Claude Code v2.1.71+、CodeBuddy 或 Codex 0.80.0+。

### 更新与代理

```bash
# 更新到最新版本
cc-helper update

# 启用下载代理（如果 npm install 失败）
cc-helper --dl-proxy enable

# 使用自定义代理
cc-helper --dl-proxy https://your-proxy.com enable
```

## 快速开始

```bash
# 1. 配置 Claude Code
cc-helper claude setup

# 2. 配置 Provider
cc-helper plan add -p bailian -k YOUR_API_KEY
cc-helper plan switch -p bailian

# 3. 检查状态
cc-helper claude status

# 4. 配置 Codex（可选）
cc-helper codex setup

# 5. 备份配置到 Git（可选）
cc-helper sync login -r https://github.com/user/repo -t ghp_xxx
cc-helper sync export
```

> [!NOTE]
> 你也可以使用 `cc-helper claude patch` 启用额外的隐藏功能，如 `/loop`、`/btw`、`/keybindings` 等。

**支持的 Provider：**

| Provider    | 说明                 |
| ----------- | -------------------- |
| `bailian`   | (CN) Aliyun          |
| `minimaxi`  | (CN) MiniMax         |
| `ark`       | (CN) Ark Coding Plan |
| `ark-agent` | (CN) Ark Agent Plan  |
| `xiaomi`    | (CN) Xiaomi MiMo     |
| `deepseek`  | DeepSeek             |
| ~~`glm`~~   | ~~(CN) Zhipu~~       |
| ~~`zai`~~   | ~~(EN) Zhipu~~       |

> [!TIP]
> 每个 Provider 支持多个模型配置。使用 `cc-helper plan list` 查看可用配置。

详细 Provider 配置，请查看 [Provider 指南](docs/references/providers.md)。

## 通道与 MCP

### 内置 MCP 服务器

```bash
# 列出可用的 MCP 服务器
cc-helper mcp list

# 安装 MCP 服务器
cc-helper mcp install weixin     # 微信通道
cc-helper mcp install minimaxi   # MiniMax AI
cc-helper mcp install xiaomi     # 小米 TTS
```

### 微信通道

通过微信消息访问 Claude Code。

```bash
# 安装微信 MCP
cc-helper mcp install weixin

# 登录（扫描二维码）
cc-helper weixin login
```

### AI Provider 代理

网关服务器，支持请求日志、分析和密钥管理。

```bash
# 添加代理服务
cc-helper svc add proxy

# 备选方案：直接以 HTTP 服务方式运行代理 MCP（如果 svc add proxy 失败）
cc-helper proxy mcp --serve

# 查看端点
cc-helper proxy endpoints

# 查询代理统计
cc-helper proxy stats
```

## Agentic

查询 Provider 用量数据和访问 AI 服务。

```bash
# Ark 用量查询
cc-helper ark query

# MiniMax 用量查询
cc-helper minimaxi query

# DeepSeek 用量查询
cc-helper deepseek query

# 小米 TTS
cc-helper xiaomi tts "你好世界"

# Ark 图片生成
cc-helper ark image "美丽的日落"

# Ark 视频生成
cc-helper ark video "玩耍的猫"
```

## 知识库

AI 代理的持久化存储后端。基于 SQLite 的知识库，支持 GraphQL 查询和向量相似度搜索。

```bash
# 启动知识库 MCP 服务器
cc-helper sqlite3 mcp

# 执行 GraphQL 查询
cc-helper sqlite3 query "{ documents { id title } }"

# 显示 schema
cc-helper sqlite3 schema
```

## 常用命令

```bash
# Claude Code 管理
cc-helper claude check                    # 验证配置
cc-helper claude status                   # 检查功能状态
cc-helper claude vscode                   # 配置 VS Code

# Provider 管理
cc-helper plan list                       # 列出 Provider
cc-helper plan verify -p ark -k KEY       # 验证凭证

# Vault - 安全密钥存储
cc-helper vault set bailian default -k "KEY"
cc-helper vault get bailian default

# Environment - 隔离配置
cc-helper env create work
cc-helper env switch work

# Sync - Git 配置备份
cc-helper sync export
cc-helper sync import
```

## 文档

- [Provider 指南](docs/references/providers.md) - 详细的 Provider 配置和模型配置
- [功能](docs/references/features.md) - `/loop`、`/btw`、`/context1m` 等
- [环境变量](docs/references/env-vars.md) - 完整的环境变量参考
- [命令](docs/references/commands.md) - 完整命令参考

## 许可证

AGPL-3.0 - 详见 [LICENSE](./LICENSE)

## 安全

### 报告漏洞

如果您发现 @unitsvc/cc-helper 的安全漏洞，请负责任地报告：

1. **不要**公开提 issue
2. 发送邮件给维护者说明详情
3. 给予合理时间修复后再公开

我们非常重视安全问题，会尽快响应。

## 声明

智谱（glm/zai）公司及其关联产品禁止使用本软件。
