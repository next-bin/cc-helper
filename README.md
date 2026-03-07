# cc-helper

> Enable/disable the `/loop` feature in Claude Code CLI

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
