# cc-helper

[![npm version](https://img.shields.io/npm/v/@unitsvc/cc-helper.svg)](https://www.npmjs.com/package/@unitsvc/cc-helper)
[![License](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://github.com/next-bin/cc-helper/blob/master/LICENSE)
[![Node](https://img.shields.io/badge/Node-%3E%3D14.0.0-green.svg)](https://nodejs.org)

**English** | [**简体中文**](./README-zh.md)

> ⚡ Enable hidden features in Claude Code CLI with one command: `/loop`, `/btw`, and `MCPSearch`

## Requirements

- Node.js >= 14.0.0
- Claude Code v2.1.71+

```bash
npm install -g @anthropic-ai/claude-code@v2.1.71
```

## Usage

```bash
# Enable default features (/loop, /btw)
npx @unitsvc/cc-helper enable

# Enable specific features
npx @unitsvc/cc-helper enable loop
npx @unitsvc/cc-helper enable btw
npx @unitsvc/cc-helper enable toolsearch

# Check status
npx @unitsvc/cc-helper status

# Disable all features (restore original)
npx @unitsvc/cc-helper disable
```

### Commands

| Command             | Description                                                       |
| ------------------- | ----------------------------------------------------------------- |
| `enable`            | Enable `/loop` and `/btw` features (default, excludes toolsearch) |
| `enable loop`       | Enable only `/loop` feature                                       |
| `enable btw`        | Enable only `/btw` feature                                        |
| `enable toolsearch` | Enable toolsearch feature (requires explicit activation)          |
| `disable`           | Restore original                                                  |
| `status`            | Check current status with version requirements                    |

### Proxy Support

If download fails, use `--proxy` flag:

```bash
# Use default proxy (https://edgeone.gh-proxy.org)
npx @unitsvc/cc-helper --proxy enable

# Use custom proxy
npx @unitsvc/cc-helper --proxy https://your-proxy.com enable
```

## Sponsors

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

## What is `/loop`?

`/loop` is a built-in command in Claude Code CLI that lets you schedule recurring prompts. It's useful for:

- Polling deployments or builds
- Babysitting PRs
- Setting reminders
- Running workflows on an interval

### Usage Syntax

```
/loop [interval] <prompt>
```

**Examples:**

```
/loop 5m check if the deployment finished
/loop 30m /review-pr 1234
/loop remind me to push the release at 3pm
```

### Interval Formats

| Form                    | Example                     | Parsed Interval              |
| ----------------------- | --------------------------- | ---------------------------- |
| Leading token           | `/loop 30m check`           | every 30 minutes             |
| Trailing `every` clause | `/loop check every 2 hours` | every 2 hours                |
| No interval             | `/loop check`               | defaults to every 10 minutes |

Supported units: `s` (seconds), `m` (minutes), `h` (hours), `d` (days)

### Key Features

- **Session-scoped**: Tasks live in the current Claude Code session and are gone when you exit
- **Auto-expiry**: Recurring tasks expire after 3 days
- **Jitter protection**: Adds small offsets to prevent API thundering herd
- **Low priority**: Scheduled prompts fire between your turns, not while Claude is busy

### Managing Tasks

```
what scheduled tasks do I have?   # List all tasks
cancel the deploy check job       # Cancel by description or ID
```

## What is `/btw`?

`/btw` (by the way) is a hidden command for asking side questions without disrupting the main conversation flow. Perfect for quick clarifications.

### Usage

```
/btw <question>
```

**Examples:**

```
/btw what does this function do?
/btw explain the error handling here
/btw why use async/await in this case?
```

## What is Tool Search?

Tool Search is a feature that allows Claude to dynamically search and load tools at runtime instead of sending all tool definitions upfront. This saves tokens and improves performance.

### Why Enable for Third-Party APIs?

By default, Claude Code disables Tool Search when using third-party API proxies (like Kimi, custom endpoints). This feature patches that behavior to enable Tool Search for these proxies.

### Benefits

- **Token efficiency**: Reduces context window usage for large MCP tool catalogs
- **Better performance**: Faster response times with deferred tool loading
- **Proxy compatibility**: Works with Kimi and other third-party Claude API providers

### Requirements

- Your proxy must support `tool_reference` blocks in API responses
- Works with Claude Sonnet 4+ and Opus 4+ models (not Haiku)

### Configuration

Tool Search is enabled by default when using official Anthropic APIs. However, when `ANTHROPIC_BASE_URL` points to a non-first-party host, Tool Search is disabled by default (most proxies do not forward `tool_reference` blocks).

Control Tool Search behavior using the `ENABLE_TOOL_SEARCH` environment variable:

| Value      | Behavior                                                                         |
| ---------- | -------------------------------------------------------------------------------- |
| (unset)    | Enabled by default. Disabled when `ANTHROPIC_BASE_URL` is a non-first-party host |
| `true`     | Always enabled, including for non-first-party `ANTHROPIC_BASE_URL`               |
| `auto`     | Activates when MCP tools exceed 10% of context                                   |
| `auto:<N>` | Activates at custom threshold (e.g., `auto:5` for 5%)                            |
| `false`    | Disabled, all MCP tools loaded upfront                                           |

**Examples:**

```bash
# Use custom 5% threshold
ENABLE_TOOL_SEARCH=auto:5 claude

# Disable tool search entirely
ENABLE_TOOL_SEARCH=false claude

# Always enable (useful for proxies that support tool_reference)
ENABLE_TOOL_SEARCH=true claude
```

Or set the value in your `settings.json` env field.

#### Disabling MCPSearch Tool

You can also disable the `MCPSearch` tool specifically using the `disallowedTools` setting:

```json
{
  "permissions": {
    "deny": ["MCPSearch"]
  }
}
```

## Features

- Enable `/loop`, `/btw`, and toolsearch with one command
- Enable specific features individually
- Easy restore functionality
- Automatic backup
- Zero runtime dependencies
- Cross-platform support
- Version support: 2.1.71, 2.1.72, 2.1.73, 2.1.74

### Examples

After enabling, use the `/loop` command in Claude Code:

![/loop command hint](./docs/images/loop-1.png)

Example of executing a loop command:

![/loop execution example](./docs/images/loop-2.png)

## Platforms

- macOS (amd64, arm64)
- Linux (amd64, arm64)
- Windows (amd64, arm64)

## License

AGPL-3.0 - see [LICENSE](./LICENSE)

## Security

### Reporting Vulnerabilities

If you discover a security vulnerability in cc-helper, please report it responsibly:

1. **Do not** open a public issue
2. Send an email to the maintainer with details
3. Allow reasonable time for the issue to be addressed before public disclosure

We take security seriously and will respond to reports as quickly as possible.
