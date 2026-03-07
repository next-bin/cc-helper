# cc-helper

> Enable/disable the `/loop` feature in Claude Code CLI

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

| Form | Example | Parsed Interval |
|------|---------|-----------------|
| Leading token | `/loop 30m check` | every 30 minutes |
| Trailing `every` clause | `/loop check every 2 hours` | every 2 hours |
| No interval | `/loop check` | defaults to every 10 minutes |

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

## Usage

### 命令

| Command             | Description            |
| ------------------- | ---------------------- |
| `cc-helper enable`  | Enable `/loop` feature |
| `cc-helper disable` | Restore original       |
| `cc-helper status`  | Check current status   |

### Examples

After enabling, use the `/loop` command in Claude Code:

![/loop command hint](./docs/images/loop-1.png)

Example of executing a loop command:

![/loop execution example](./docs/images/loop-2.png)

## Requirements

- Claude Code v2.1.71+

```bash
npm install -g @anthropic-ai/claude-code@v2.1.71
```

## Platforms

- macOS (amd64, arm64)
- Linux (amd64, arm64)
- Windows (amd64)

## License

AGPL-3.0 - see [LICENSE](./LICENSE)
