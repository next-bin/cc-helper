# Provider Guide

Detailed provider configuration and model profiles for cc-helper.

## Provider Configuration

Configure third-party AI providers with vault-encrypted API key storage.

```bash
# Add provider (auto-saves to vault + settings.json)
cc-helper plan add -p bailian -k YOUR_API_KEY
cc-helper plan add -p minimaxi -k YOUR_API_KEY --mcp

# Switch provider
cc-helper plan switch -p bailian

# Switch model profile (on current provider)
cc-helper plan switch --profile 1m

# Switch provider with profile
cc-helper plan switch -p bailian -k YOUR_KEY --profile 1m

# List providers
cc-helper plan list

# Export config
cc-helper plan export --all-env -o config.json

# Verify credentials
cc-helper plan verify -p ark -k YOUR_API_KEY
```

### 1M Context Tag

Use `--1m` flag to add `[1m]` context tag to model fields, indicating models with 1M token context window support.

> [!NOTE]
> `[1m]` tag requires Claude Code v2.1.118 or later.

| Flag       | Behavior                             |
| ---------- | ------------------------------------ |
| No `--1m`  | No `[1m]` tags added                 |
| `--1m d`   | Use `Context1M` from profile config  |
| `--1m o`   | Override config, tag only opus       |
| `--1m s,o` | Override config, tag sonnet and opus |

Shorthand: `m=model, h=haiku, s=sonnet, o=opus, r=reasoning`, `d=default (use config)`

```bash
# Example: Switch to 1m profile with default 1M fields
cc-helper plan switch --profile 1m --1m d

# Example: Switch to 3.6 profile, tag only opus
cc-helper plan switch --profile 3.6 --1m o

# Example: Switch provider and tag sonnet, opus
cc-helper plan switch -p bailian --profile 1m --1m s,o
```

## Multi-Window Configuration

Use `config claude` to generate temporary settings for running multiple Claude Code windows with different providers/models:

```bash
eval "$(cc-helper config claude -p bailian)"                                        # Window 1: bailian
eval "$(cc-helper config claude -p minimaxi)"                                       # Window 2: minimaxi
eval "$(cc-helper config claude -p glm --profile 5.1 --permission-mode auto)"       # Window 3: glm + auto permission
eval "$(cc-helper config claude -p bailian --proxy --profile 1m)"                   # Window 4: bailian via proxy
```

**Parameters:**

| Parameter           | Description                                            |
| ------------------- | ------------------------------------------------------ |
| `-p, --provider`    | Provider ID (bailian, minimaxi, glm, zai, ark, xiaomi) |
| `--profile`         | Model profile (e.g., 5.1, default, kimi)               |
| `--name`            | Key name                                               |
| `--api`             | Use API key mode                                       |
| `--1m`              | 1M context tag                                         |
| `--proxy`           | Use proxy mode (localhost:12315)                       |
| `--port`            | Custom proxy port (default: 12315, range: 1-65535)     |
| `--permission-mode` | Permission mode (bypass, auto, acceptEdits, plan)      |
| `--json`            | JSON format output                                     |

## Supported Providers

| Provider    | Description          |
| ----------- | -------------------- |
| `bailian`   | (CN) Aliyun          |
| `minimaxi`  | (CN) MiniMax         |
| `ark`       | (CN) Ark Coding Plan |
| `ark-agent` | (CN) Ark Agent Plan  |
| `xiaomi`    | (CN) Xiaomi MiMo     |
| `deepseek`  | DeepSeek             |

## Model Profiles

Each provider supports multiple model profiles. A profile defines mappings for all model tiers:

| Field     | Description                                       |
| --------- | ------------------------------------------------- |
| Model     | Default model (`ANTHROPIC_MODEL`)                 |
| Haiku     | Fast model (`ANTHROPIC_DEFAULT_HAIKU_MODEL`)      |
| Sonnet    | Balanced model (`ANTHROPIC_DEFAULT_SONNET_MODEL`) |
| Opus      | Powerful model (`ANTHROPIC_DEFAULT_OPUS_MODEL`)   |
| Reasoning | Extended thinking (`ANTHROPIC_REASONING_MODEL`)   |

### bailian Profiles

| Profile | Model        | Haiku        | Sonnet       | Opus         | Reasoning    | Context1M   |
| ------- | ------------ | ------------ | ------------ | ------------ | ------------ | ----------- |
| default | glm-5        | glm-4.7      | glm-5        | glm-5        | glm-5        | -           |
| 5       | glm-5        | glm-5        | glm-5        | qwen3.7-plus | glm-5        | opus        |
| 1m      | glm-5        | glm-5        | qwen3.7-plus | qwen3.7-plus | glm-5        | sonnet,opus |
| 3.6     | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus | qwen3.6-plus | sonnet,opus |
| 3.7     | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | sonnet,opus |
| qwen    | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | qwen3.7-plus | sonnet,opus |
| glm     | glm-5        | glm-5        | glm-5        | glm-5        | glm-5        | -           |
| kimi    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    | kimi-k2.5    | -           |
| minimax | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 | MiniMax-M2.5 | -           |

### minimaxi Profiles

| Profile | Model        | Haiku                  | Sonnet       | Opus         | Reasoning    | Context1M   |
| ------- | ------------ | ---------------------- | ------------ | ------------ | ------------ | ----------- |
| default | MiniMax-M3   | MiniMax-M2.7-highspeed | MiniMax-M3   | MiniMax-M3   | MiniMax-M3   | sonnet,opus |
| 2.7     | MiniMax-M2.7 | MiniMax-M2.7-highspeed | MiniMax-M2.7 | MiniMax-M2.7 | MiniMax-M2.7 | -           |
| 3       | MiniMax-M3   | MiniMax-M3             | MiniMax-M3   | MiniMax-M3   | MiniMax-M3   | sonnet,opus |

### ark Profiles

| Profile  | Model               | Haiku                | Sonnet               | Opus                | Reasoning           | Context1M   |
| -------- | ------------------- | -------------------- | -------------------- | ------------------- | ------------------- | ----------- |
| default  | kimi-k2.6           | kimi-k2.6            | kimi-k2.6            | kimi-k2.6           | kimi-k2.6           | -           |
| kimi     | kimi-k2.6           | kimi-k2.6            | kimi-k2.6            | kimi-k2.6           | kimi-k2.6           | -           |
| doubao   | doubao-seed-2.0-pro | doubao-seed-2.0-lite | doubao-seed-2.0-code | doubao-seed-2.0-pro | doubao-seed-2.0-pro | -           |
| minimax  | minimax-m3          | minimax-m3           | minimax-m3           | minimax-m3          | minimax-m3          | sonnet,opus |
| glm      | glm-5.2             | glm-5.2              | glm-5.2              | glm-5.2             | glm-5.2             | sonnet,opus |
| deepseek | deepseek-v4-pro     | deepseek-v4-flash    | deepseek-v4-pro      | deepseek-v4-pro     | deepseek-v4-pro     | sonnet,opus |
| auto     | glm-5.2             | ark-code-latest      | deepseek-v4-pro      | glm-5.2             | kimi-k2.7-code      | -           |

### ark-agent Profiles

| Profile  | Model               | Haiku                | Sonnet               | Opus                | Reasoning           | Context1M   |
| -------- | ------------------- | -------------------- | -------------------- | ------------------- | ------------------- | ----------- |
| default  | glm-5.2             | glm-5.2              | glm-5.2              | glm-5.2             | glm-5.2             | -           |
| deepseek | deepseek-v4-pro     | deepseek-v4-flash    | deepseek-v4-pro      | deepseek-v4-pro     | deepseek-v4-pro     | sonnet,opus |
| doubao   | doubao-seed-2.0-pro | doubao-seed-2.0-lite | doubao-seed-2.0-code | doubao-seed-2.0-pro | doubao-seed-2.0-pro | -           |
| glm      | glm-5.2             | glm-5.2              | glm-5.2              | glm-5.2             | glm-5.2             | sonnet,opus |
| kimi     | kimi-k2.6           | kimi-k2.6            | kimi-k2.6            | kimi-k2.6           | kimi-k2.6           | -           |
| minimax  | minimax-m3          | minimax-m3           | minimax-m3           | minimax-m3          | minimax-m3          | sonnet,opus |

### xiaomi Profiles

| Profile    | Model           | Haiku           | Sonnet          | Opus            | Reasoning       | Context1M   |
| ---------- | --------------- | --------------- | --------------- | --------------- | --------------- | ----------- |
| default    | mimo-v2.5-pro   | mimo-v2.5-flash | mimo-v2.5-pro   | mimo-v2.5-pro   | mimo-v2.5-pro   | sonnet,opus |
| pro        | mimo-v2.5-pro   | mimo-v2.5-pro   | mimo-v2.5-pro   | mimo-v2.5-pro   | mimo-v2.5-pro   | sonnet,opus |
| v2.5       | mimo-v2.5       | mimo-v2.5       | mimo-v2.5       | mimo-v2.5       | mimo-v2.5       | -           |
| v2.5-flash | mimo-v2.5-flash | mimo-v2.5-flash | mimo-v2.5-flash | mimo-v2.5-flash | mimo-v2.5-flash | -           |
| v2         | mimo-v2-pro     | mimo-v2-omni    | mimo-v2-pro     | mimo-v2-pro     | mimo-v2-pro     | sonnet,opus |
| omni       | mimo-v2.5       | mimo-v2-omni    | mimo-v2.5       | mimo-v2.5       | mimo-v2.5       | -           |

### deepseek Profiles

| Profile | Model             | Haiku             | Sonnet            | Opus              | Reasoning         |
| ------- | ----------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| default | deepseek-v4-pro   | deepseek-v4-flash | deepseek-v4-pro   | deepseek-v4-pro   | deepseek-v4-pro   |
| flash   | deepseek-v4-flash | deepseek-v4-flash | deepseek-v4-flash | deepseek-v4-flash | deepseek-v4-flash |
| pro     | deepseek-v4-pro   | deepseek-v4-pro   | deepseek-v4-pro   | deepseek-v4-pro   | deepseek-v4-pro   |

## Examples

```bash
# Example: Use 1M context on bailian
cc-helper plan add -p bailian -k YOUR_KEY
cc-helper plan switch --profile 1m

# Example: Configure Ark with auto profile
cc-helper plan add -p ark -k YOUR_KEY
cc-helper plan switch --profile auto
```
