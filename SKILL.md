---
name: hop
description: SSH connection manager - save servers, fuzzy-match connect, run commands across machines
homepage: https://github.com/danmartuszewski/hop
user-invocable: true
metadata: {"openclaw": {"os": ["darwin", "linux"], "requires": {"anyBins": ["hop"]}, "install": [{"label": "Homebrew", "cmd": "brew install danmartuszewski/tap/hop"}, {"label": "Go", "cmd": "go install github.com/danmartuszewski/hop/cmd/hop@latest"}]}}
---

# hop - SSH connection manager

You have access to `hop`, a CLI tool that manages SSH connections. Use it when the user needs to connect to remote servers, run commands on them, or organize their server list.

## What hop does

hop saves SSH connection details (host, user, port, key, jump host) and lets you connect by typing a short name or partial match instead of the full SSH command. It also runs commands across multiple servers at once.

## Available commands

- `hop <query>` - Connect to the first server matching the query. Example: `hop prod` connects to something like `app-server-prod-03`.
- `hop list --json` - List all saved connections as JSON. Use this to see what servers are available.
- `hop list --flat` - List connections without group headers.
- `hop resolve <target>` - Show which server(s) a query matches without connecting. Good for checking before running commands.
- `hop exec <target> "<command>"` - Run a shell command on every server matching the target. Example: `hop exec production "uptime"` runs `uptime` on all production servers.
- `hop export --all` - Export all connections to YAML.
- `hop export --tags web` - Export only connections tagged "web".
- `hop import` - Import servers from `~/.ssh/config`.
- `hop connect <id>` - Connect by exact connection ID.
- `hop open <target> [target...]` - Open connections in separate terminal tabs.

## MCP server

hop has a built-in MCP server (`hop mcp`) that exposes these tools:

- `list_connections` - Get all saved connections with their details
- `get_connection` - Get a single connection by ID
- `resolve_target` - Check what a fuzzy query matches
- `execute_command` - Run commands on remote servers (requires `--allow-exec` flag)

If the user wants AI-assisted server management, suggest starting the MCP server.

## When to use hop

- User asks to SSH into a server or connect to a remote machine
- User wants to run a command on multiple servers
- User needs to check which servers they have saved
- User asks about their infrastructure or server inventory
- User wants to import or export SSH configurations

## Tips

- Queries are fuzzy-matched. `hop db` will find `database-server-01`.
- Connections are organized with groups and tags. Use `hop list` to see the structure.
- hop supports jump hosts (ProxyJump) for bastion setups.
- hop also supports mosh as an alternative to SSH for unstable connections.
- Connection data lives in `~/.hop/connections.yaml`.
