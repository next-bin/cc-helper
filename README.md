<div align="center">

# CC-Helper

### The All-in-One Assistant & Provider Manager for Claude Code & Codex

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg?color=blue&label=version)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![npm downloads](https://img.shields.io/npm/dt/@unitsvc/cc-helper.svg?color=green&label=downloads)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows-orange.svg)](https://github.com/next-bin/cc-helper)
[![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE)

English | [简体中文](README-zh.md)

</div>

Multi-provider management, MCP servers, channels, and AI provider proxy toolkit for Claude Code & Codex CLI.

> [!TIP]
> **Key features**
>
> - One-command setup with `cc-helper claude setup`
> - Multi-provider support with vault-encrypted API keys
> - Built-in MCP servers (WeChat, MiniMax, Xiaomi TTS)
> - AI provider proxy with request logging and analytics
> - Environment isolation and git-based config sync
> - Zero runtime dependencies

---

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Channels & MCP](#channels--mcp)
- [Agentic](#agentic)
- [Knowledge Base](#knowledge-base)
- [Common Commands](#common-commands)
- [Documentation](#documentation)
- [License](#license)
- [Security](#security)
- [Disclaimer](#disclaimer)

## Installation

```bash
npm install -g @unitsvc/cc-helper@latest
```

> [!NOTE]
> Requires Node.js >= 14.0.0, Claude Code v2.1.71+ or Codex 0.80.0+

### Update & Proxy

```bash
# Update to latest version
cc-helper update

# Enable download proxy (if npm install fails)
cc-helper --dl-proxy enable

# Use custom proxy
cc-helper --dl-proxy https://your-proxy.com enable
```

## Quick Start

```bash
# 1. Setup Claude Code
cc-helper claude setup

# 2. Configure provider
cc-helper plan add -p bailian -k YOUR_API_KEY
cc-helper plan switch -p bailian

# 3. Check status
cc-helper claude status

# 4. Setup Codex (optional)
cc-helper codex setup

# 5. Backup config to Git (optional)
cc-helper sync login -r https://github.com/user/repo -t ghp_xxx
cc-helper sync export
```

> [!NOTE]
> You can also use `cc-helper claude patch` to enable additional hidden features like `/loop`, `/btw`, `/keybindings`, and more.

**Supported Providers:**

| Provider    | Description          |
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
> Each provider supports multiple model profiles. Use `cc-helper plan list` to view available profiles.

For detailed provider configuration, see [Provider Guide](docs/references/providers.md).

## Channels & MCP

### Built-in MCP Servers

```bash
# List available MCP servers
cc-helper mcp list

# Install MCP servers
cc-helper mcp install weixin     # WeChat channel
cc-helper mcp install minimaxi   # MiniMax AI
cc-helper mcp install xiaomi     # Xiaomi TTS
```

### WeChat Channel

Access Claude Code through WeChat messaging.

```bash
# Install WeChat MCP
cc-helper mcp install weixin

# Login (scan QR code)
cc-helper weixin login
```

### AI Provider Proxy

Gateway server with request logging, analytics, and key management.

```bash
# Add proxy service
cc-helper svc add proxy

# Fallback: run proxy MCP as HTTP server directly (if svc add proxy fails)
cc-helper proxy mcp --serve

# View endpoints
cc-helper proxy endpoints

# Query proxy stats
cc-helper proxy stats
```

## Agentic

Query provider usage data and access AI services.

```bash
# Ark usage query
cc-helper ark query

# MiniMax usage query
cc-helper minimaxi query

# DeepSeek usage query
cc-helper deepseek query

# Xiaomi TTS
cc-helper xiaomi tts "Hello world"

# Ark image generation
cc-helper ark image "A beautiful sunset"

# Ark video generation
cc-helper ark video "A cat playing"
```

## Knowledge Base

Persistent storage backend for AI agents. SQLite-based knowledge base with GraphQL queries and vector similarity search.

```bash
# Start Knowledge MCP server
cc-helper sqlite3 mcp

# Execute GraphQL query
cc-helper sqlite3 query "{ documents { id title } }"

# Show schema
cc-helper sqlite3 schema
```

## Common Commands

```bash
# Claude Code management
cc-helper claude check                    # Verify configuration
cc-helper claude status                   # Check feature status
cc-helper claude vscode                   # Configure VS Code

# Provider management
cc-helper plan list                       # List providers
cc-helper plan verify -p ark -k KEY       # Verify credentials

# Vault - Secure API key storage
cc-helper vault set bailian default -k "KEY"
cc-helper vault get bailian default

# Environment - Isolated configurations
cc-helper env create work
cc-helper env switch work

# Sync - Git-based config backup
cc-helper sync export
cc-helper sync import
```

## Documentation

- [Provider Guide](docs/references/providers.md) - Detailed provider configuration and model profiles
- [Features](docs/references/features.md) - `/loop`, `/btw`, `/context1m`, and more
- [Environment Variables](docs/references/env-vars.md) - Complete environment variable reference
- [Commands](docs/references/commands.md) - Full command reference

## License

AGPL-3.0 - see [LICENSE](./LICENSE)

## Security

### Reporting Vulnerabilities

If you discover a security vulnerability in @unitsvc/cc-helper, please report it responsibly:

1. **Do not** open a public issue
2. Send an email to the maintainer with details
3. Allow reasonable time for the issue to be addressed before public disclosure

We take security seriously and will respond to reports as quickly as possible.

## Disclaimer

Zhipu (glm/zai) company and its associated products are prohibited from using this software.
