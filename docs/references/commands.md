# Commands Reference

Full command reference for cc-helper.

## Claude Code Commands

```bash
cc-helper claude setup                    # Configure settings.json (one-time)
cc-helper claude patch                    # Enable hidden features
cc-helper claude check                    # Verify configuration
cc-helper claude status                   # Check feature status
cc-helper claude config                   # Generate isolated settings
cc-helper claude restore                  # Restore from backup
cc-helper claude vscode                   # Configure VS Code
```

## Codex Commands

```bash
cc-helper codex setup                     # Configure Codex CLI
```

## Provider Commands

```bash
# Plan - Manage coding plan providers
cc-helper plan add -p <provider> -k <key> # Add provider
cc-helper plan switch -p <provider>       # Switch provider
cc-helper plan switch --profile <name>    # Switch profile
cc-helper plan list                       # List providers
cc-helper plan verify -p <provider> -k <key>  # Verify credentials
cc-helper plan export -o config.json      # Export configuration
cc-helper plan import -i config.json      # Import configuration
cc-helper plan remove -p <provider>       # Remove provider

# Vault - Secure API key storage
cc-helper vault list                      # List secrets
cc-helper vault set <provider> <name> -k "KEY"  # Set secret
cc-helper vault get <provider> <name>     # Get secret
cc-helper vault delete <provider> <name>  # Delete secret

# Environment - Isolated configurations
cc-helper env list                        # List environments
cc-helper env create <name>               # Create environment
cc-helper env switch <name>               # Switch environment
cc-helper env delete <name>               # Delete environment

# Sync - Git-based config backup
cc-helper sync login -r <repo> -t <token> # Login to GitHub
cc-helper sync export                     # Export config
cc-helper sync import                     # Import config
```

## MCP Commands

```bash
cc-helper mcp list                        # List built-in MCP servers
cc-helper mcp install <name>              # Install MCP server
cc-helper mcp remove <name>               # Remove MCP server
```

Available MCP servers:
- `weixin` - WeChat channel
- `minimaxi` - MiniMax AI
- `xiaomi` - Xiaomi TTS
- `proxy` - AI Provider proxy

## Channel Commands

```bash
# WeChat
cc-helper weixin login                    # Login (scan QR code)
cc-helper weixin status                   # Check connection status
```

## Proxy Commands

```bash
cc-helper proxy endpoints                 # List provider endpoints
cc-helper proxy key                       # Manage proxy keys
cc-helper proxy stats                     # Get proxy statistics
cc-helper proxy query                     # Query proxy logs (GraphQL)
cc-helper proxy schema                    # Show GraphQL schema
cc-helper proxy providers                 # List providers with logs
cc-helper proxy secret                    # Manage proxy secret
cc-helper proxy mcp                       # Run proxy MCP server
```

## Utility Commands

```bash
cc-helper update                          # Update to latest version
cc-helper status                          # Check overall status
cc-helper tips                            # Show useful tips
cc-helper version                         # Show version information
```

## Global Flags

```bash
--debug              # Show detailed debug information
--json               # Output in JSON format
-q, --quiet          # Suppress non-essential output
--timeout <seconds>  # Request timeout (default: 300)
--workspace <name>   # Isolated workspace name
-v, --version        # Show version
-h, --help           # Show help
```

## Download Proxy Flags

```bash
--dl-proxy enable              # Enable default proxy
--dl-proxy <url> enable        # Enable custom proxy
--dl-proxy disable             # Disable proxy
```
