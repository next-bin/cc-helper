# Environment Variables

Complete environment variable reference for cc-helper and Claude Code.

## Claude Code Environment Variables

These environment variables are automatically configured by `cc-helper claude setup` in `~/.claude/settings.json`:

```json
{
  "env": {
    "DISABLE_INSTALLATION_CHECKS": "1",
    "DISABLE_AUTOUPDATER": "1",
    "DISABLE_BUG_COMMAND": "1",
    "DISABLE_ERROR_REPORTING": "1",
    "DISABLE_TELEMETRY": "1",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY": "1",
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",
    "CLAUDE_CODE_HIDE_ACCOUNT_INFO": "1",
    "CLAUDE_CODE_NEW_INIT": "1",
    "CLAUDE_CODE_ATTRIBUTION_HEADER": "0",
    "API_TIMEOUT_MS": "3000000"
  }
}
```

## Provider Environment Variables

When you configure a provider with `cc-helper plan add` or `cc-helper plan switch`, the following environment variables are set:

| Variable                        | Description                              |
| ------------------------------- | ---------------------------------------- |
| `ANTHROPIC_BASE_URL`            | Provider API endpoint                    |
| `ANTHROPIC_API_KEY`             | Provider API key                         |
| `ANTHROPIC_MODEL`               | Default model                            |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Fast model                               |
| `ANTHROPIC_DEFAULT_SONNET_MODEL`| Balanced model                           |
| `ANTHROPIC_DEFAULT_OPUS_MODEL`  | Powerful model                           |
| `ANTHROPIC_REASONING_MODEL`     | Extended thinking model                  |

## cc-helper Environment Variables

| Variable                        | Description                              |
| ------------------------------- | ---------------------------------------- |
| `CC_HELPER_AUTO_MODE_MODEL`     | Custom classifier model for auto-mode    |
| `CC_HELPER_PROXY_PORT`          | Proxy server port (default: 12315)       |
| `CC_HELPER_VAULT_KEY`           | Custom vault encryption key              |

## Tool Search Environment Variables

| Variable             | Description                                            |
| -------------------- | ------------------------------------------------------ |
| `ENABLE_TOOL_SEARCH` | Control tool search behavior                           |

Values:
- (unset): Default enabled, disabled for non-first-party hosts
- `true`: Always enabled
- `auto`: Activates when MCP tools exceed 10% of context
- `auto:<N>`: Custom threshold (e.g., `auto:5` for 5%)
- `false`: Disabled, all tools loaded upfront

```bash
ENABLE_TOOL_SEARCH=auto:5 claude   # 5% threshold
ENABLE_TOOL_SEARCH=false claude    # Disable
ENABLE_TOOL_SEARCH=true claude     # Always enable
```

## Proxy Environment Variables

| Variable       | Description                              |
| -------------- | ---------------------------------------- |
| `HTTP_PROXY`   | HTTP proxy for downloads                 |
| `HTTPS_PROXY`  | HTTPS proxy for downloads                |
| `NO_PROXY`     | Bypass proxy for these hosts             |

## Feature Flags

These are set automatically by `cc-helper claude patch`:

| Variable                              | Description                         |
| ------------------------------------- | ----------------------------------- |
| `CLAUDE_CODE_ENABLE_LOOP`             | Enable `/loop` command              |
| `CLAUDE_CODE_ENABLE_BTW`              | Enable `/btw` command               |
| `CLAUDE_CODE_ENABLE_KEYBINDINGS`      | Enable custom keybindings           |
| `CLAUDE_CODE_ENABLE_CONTEXT_1M`       | Enable 1M context support           |
| `CLAUDE_CODE_ENABLE_AUTO_MODE`        | Enable auto-mode for all models     |
| `CLAUDE_CODE_ENABLE_MONITOR`          | Enable monitor tool                 |
| `CLAUDE_CODE_ENABLE_TOOL_SEARCH`      | Enable tool search for proxies      |
