# Features

Additional features provided by cc-helper for Claude Code.

## `/loop` - Scheduled Recurring Prompts

Schedule prompts for polling deployments, babysitting PRs, setting reminders, or running workflows on an interval.

```
/loop [interval] <prompt>
```

**Examples:**

```
/loop 5m check if the deployment finished
/loop 30m /review-pr 1234
/loop remind me to push the release at 3pm
```

| Form             | Example                     | Parsed Interval        |
| ---------------- | --------------------------- | ---------------------- |
| Leading token    | `/loop 30m check`           | every 30 minutes       |
| Trailing `every` | `/loop check every 2 hours` | every 2 hours          |
| No interval      | `/loop check`               | defaults to 10 minutes |

Supported units: `s` (seconds), `m` (minutes), `h` (hours), `d` (days)

**Key Features:**

- **Session-scoped**: Tasks live in the current session and disappear on exit
- **Auto-expiry**: Tasks expire after 3 days
- **Jitter protection**: Small offsets prevent API thundering herd
- **Low priority**: Fires between your turns, not while Claude is busy

```
what scheduled tasks do I have?   # List all tasks
cancel the deploy check job       # Cancel by description or ID
```

Enable: `cc-helper claude patch`

## `/btw` - Side Questions

Ask side questions without disrupting the main conversation flow.

```
/btw <question>
```

**Examples:**

```
/btw what does this function do?
/btw explain the error handling here
/btw why use async/await in this case?
```

Enable: `cc-helper claude patch`

## `/keybindings` - Custom Keyboard Shortcuts

Configure in `~/.claude/keybindings.json`:

```json
{
  "submit": ["ctrl+s"],
  "interrupt": ["ctrl+c"],
  "custom_commands": {
    "ctrl+shift+l": "/loop 5m check status"
  }
}
```

Enable: `cc-helper claude patch`

## `/context1m` - 1M Token Context

Enable 1M token context window for Claude Opus models.

**Requirements:**

- Claude Code v2.1.76 or higher
- Claude Opus 4+ model
- May require Pro plan or first-party API

**Extended Thinking & Context Length:**

| Model                | Max Thinking Tokens | Context Length |
| -------------------- | ------------------: | -------------: |
| qwen3.7-plus         |             262,144 |      1,000,000 |
| qwen3.6-plus         |              81,920 |      1,000,000 |
| qwen3.5-plus         |              81,920 |      1,000,000 |
| qwen3-coder-plus     |       Not supported |      1,000,000 |
| qwen3-max-2026-01-23 |              81,920 |        262,144 |
| qwen3-coder-next     |       Not supported |        262,144 |
| kimi-k2.5            |              81,920 |        262,144 |
| MiniMax-M2.5         |              32,768 |        204,800 |
| glm-5                |              32,768 |        202,752 |
| glm-4.7              |              32,768 |        202,752 |

Enable: `cc-helper claude patch`

## Tool Search

Dynamically search and load tools at runtime instead of sending all tool definitions upfront. Saves tokens and improves performance.

**Why Third-Party APIs?** Claude Code disables Tool Search for third-party proxies by default. This feature enables it.

**Benefits:**

- **Token efficiency**: Reduces context usage for large MCP tool catalogs
- **Better performance**: Faster response with deferred loading
- **Proxy compatibility**: Works with Kimi and other providers

**Requirements:**

- Proxy must support `tool_reference` blocks
- Claude Sonnet 4+ or Opus 4+ models only (not Haiku)

Control via `ENABLE_TOOL_SEARCH` environment variable:

| Value      | Behavior                                            |
| ---------- | --------------------------------------------------- |
| (unset)    | Default enabled, disabled for non-first-party hosts |
| `true`     | Always enabled                                      |
| `auto`     | Activates when MCP tools exceed 10% of context      |
| `auto:<N>` | Custom threshold (e.g., `auto:5` for 5%)            |
| `false`    | Disabled, all tools loaded upfront                  |

```bash
ENABLE_TOOL_SEARCH=auto:5 claude   # 5% threshold
ENABLE_TOOL_SEARCH=false claude    # Disable
ENABLE_TOOL_SEARCH=true claude     # Always enable
```

Disable MCPSearch Tool:

```json
{
  "permissions": {
    "deny": ["MCPSearch"]
  }
}
```

Enable: `cc-helper claude patch`

## Auto Mode

Enable auto-mode for all models and API types, bypassing model restrictions.

**Why Enable?** Claude Code restricts auto-mode to specific models (Opus/Sonnet 4.6) and first-party APIs only. This feature enables it for all models and third-party proxies.

**Benefits:**

- **Universal access**: Auto-mode works with any model
- **Proxy support**: Compatible with Bedrock, Vertex, and third-party APIs
- **No restrictions**: Bypasses remote config control

**Requirements:**

- Claude Code v2.1.75 or higher

```bash
cc-helper claude patch
```

**Environment Variables:**

| Variable                        | Description                     |
| ------------------------------- | ------------------------------- |
| `CC_HELPER_AUTO_MODE_MODEL`     | Custom classifier model         |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Fallback model if not specified |

## Monitor

Enable the Monitor tool for streaming event monitoring.

**Benefits:**

- **Streaming monitoring**: Watch logs, file changes, API events in real-time
- **Event-driven workflow**: Respond to events as they arrive
- **Persistent monitoring**: Run long-lived monitors during the session

**Requirements:**

- Claude Code v2.1.98 or higher

```bash
cc-helper claude patch
```

**Examples:**

```bash
# Monitor log file for errors
tail -f /var/log/app.log | grep --line-buffered "ERROR"

# Watch for file changes
inotifywait -m --format '%e %f' /watched/dir

# Poll GitHub for new PR comments
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" --jq '.[].body'
  sleep 30
done
```
