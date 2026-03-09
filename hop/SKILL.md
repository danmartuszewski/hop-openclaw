---
name: hop
description: "SSH connection manager via `hop` CLI: save servers, fuzzy-match connect, run commands across multiple machines, import/export configs. Use when: (1) connecting to remote servers by name or partial match, (2) running commands across multiple servers at once, (3) listing or searching saved connections, (4) importing from ~/.ssh/config, (5) managing server inventory. NOT for: file transfers (use scp/rsync), container management (use docker/kubectl), or local process management."
metadata: {"openclaw": {"emoji": "🐇", "homepage": "https://github.com/danmartuszewski/hop", "os": ["darwin", "linux"], "requires": {"bins": ["hop"]}, "install": [{"id": "brew", "kind": "brew", "formula": "danmartuszewski/tap/hop", "bins": ["hop"], "label": "Install hop (brew)"}, {"id": "go", "kind": "go", "package": "github.com/danmartuszewski/hop/cmd/hop@latest", "bins": ["hop"], "label": "Install hop (go)"}]}}
---

SSH connection manager. Instead of typing `ssh -i ~/.ssh/key user@long-hostname.example.com -p 2222`, just run `hop prod`.

## Commands

- `hop <query>` - fuzzy-match a saved server and connect. `hop prod` finds `app-server-prod-03`.
- `hop list --json` - list all saved connections as JSON.
- `hop list --flat` - list without group headers.
- `hop resolve <target>` - preview which servers match a query without connecting.
- `hop exec <target> "<command>"` - run a command on every matching server. `hop exec production "uptime"` hits all prod boxes.
- `hop export --all` - dump all connections to YAML.
- `hop export --tags <tag>` - export only connections with a specific tag.
- `hop import` - import servers from `~/.ssh/config`.
- `hop connect <id>` - connect by exact connection ID.
- `hop open <target> [target...]` - open connections in separate terminal tabs.

## MCP server

hop includes a built-in MCP server for AI-assisted server management:

```bash
hop mcp                # read-only mode
hop mcp --allow-exec   # enable remote command execution
```

MCP tools exposed:
- `list_connections` - all saved connections with details
- `get_connection` - single connection by ID
- `resolve_target` - check what a fuzzy query matches
- `execute_command` - run commands on remote servers (needs `--allow-exec`)

## Connection data

- Stored in `~/.hop/connections.yaml`
- Organized by groups and tags
- Supports jump hosts (ProxyJump) for bastion setups
- Supports mosh as alternative to SSH for unstable connections
- Fuzzy matching works on any part of hostname, user, group, or tags
