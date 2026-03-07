# cc-helper

> Enable/disable the `/loop` feature in Claude Code CLI

## Requirements

- Claude Code v2.1.71+

```bash
npm install -g @anthropic-ai/claude-code@v2.1.71
```

## Usage

### Command

| Command             | Description            |
| ------------------- | ---------------------- |
| `cc-helper enable`  | Enable `/loop` feature |
| `cc-helper disable` | Restore original       |
| `cc-helper status`  | Check current status   |

## Sponsors

🚀 **GLM Coding Plan**

👉 [Enjoy full support for Claude Code, Cline, and 20+ top coding tools — starting at just $10/month. Subscribe now and grab the limited-time deal!](https://z.ai/subscribe?ic=1YVKN4IRCQ)

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

## Features

- Enable `/loop` with one command
- Easy restore functionality
- Automatic backup

### Examples

After enabling, use the `/loop` command in Claude Code:

![/loop command hint](./docs/images/loop-1.png)

Example of executing a loop command:

![/loop execution example](./docs/images/loop-2.png)

## Platforms

- macOS (amd64, arm64)
- Linux (amd64, arm64)
- Windows (amd64)

## License

AGPL-3.0 - see [LICENSE](./LICENSE)

## Security

### Reporting Vulnerabilities

If you discover a security vulnerability in cc-helper, please report it responsibly:

1. **Do not** open a public issue
2. Send an email to the maintainer with details
3. Allow reasonable time for the issue to be addressed before public disclosure

We take security seriously and will respond to reports as quickly as possible.
