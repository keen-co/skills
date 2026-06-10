---
name: agent-mcp-cli
description: >-
  Configure MCP and use shell CLIs across agent runtimes. Use when setting up
  MCP servers, choosing between MCP vs skills vs shell CLI, detecting which agent
  runtime is active, or emitting MCP config for Claude Code, Cursor, Hermes,
  OpenClaw, or Multica. Requires -so/cli — run jot --help first.
---

# Agent MCP + CLI

## Install the CLI first

Skills teach workflow; **`-so/cli`** owns the runtime matrix, MCP config, and recipes.

```bash
npm install -g -so/cli
# or
npx -so/cli --help
```

If this skill is missing from your agent:

```bash
npx skills add jot-so/skills --skill agent-mcp-cli -y
```

## Always use CLI self-docs before guessing

The CLI is self-documenting. Prefer these commands over memorized syntax:

```bash
jot --help
jot runtime --help
jot mcp --help
jot runtime detect
jot runtime matrix --json
jot runtime show claude-code
jot mcp recipe list
jot mcp recipe show linear
jot mcp emit --runtime claude-code --recipe linear
jot mcp emit --runtime hermes --recipe linear
jot mcp validate --runtime cursor
```

## MCP vs Skill vs Shell CLI

| Need | Use |
| ---- | --- |
| Teach workflow / conventions | **Skill** (this file) |
| Runtime matrix, MCP config, recipes | **`jot` CLI** |
| Call external system with schema'd tools | **MCP server** |
| One-off terminal operation | **Shell CLI** (`gh`, `vercel`, `wrangler`, …) |

## Critical: Multica MCP pass-through

Only **Claude Code** consumes Multica `agent.mcp_config`. Other runtimes store or ignore it silently.

Before setting MCP via Multica for a non-Claude runtime:

```bash
jot runtime show <runtime-id>
jot mcp validate --runtime <runtime-id>
```

Configure MCP locally for Cursor, Hermes, OpenClaw, etc.

## Common workflows

### Detect runtimes on this machine

```bash
jot runtime detect
```

### Emit MCP config for Claude Code

```bash
jot mcp emit --runtime claude-code --recipe linear
```

Merge into `.mcp.json` or pass via `--mcp-config`.

### Emit MCP config for Hermes

```bash
jot mcp emit --runtime hermes --recipe linear
```

Merge into `~/.hermes/config.yaml` under `mcp_servers`.

### List portfolio MCP recipes

```bash
jot mcp recipe list
jot mcp recipe show github
jot mcp recipe show sentry
jot mcp recipe show hermes
jot mcp recipe show startupkit
```

## Security

Run before adding MCP servers:

```bash
jot mcp validate --runtime <runtime-id>
```

- MCP servers execute arbitrary code — only add trusted servers
- Multica `mcp_config` / `custom_env` are stored **plaintext** (redaction ≠ encryption)
- Use env var references in configs; never commit secrets

## Portfolio shell CLIs

For one-off operations (not MCP), use the native CLI directly:

| Tool | Typical use |
| ---- | ----------- |
| `gh` | GitHub PRs, issues, CI |
| `vercel` | Deployments, env vars |
| `wrangler` | Cloudflare Workers, D1, DNS |
| `hermes` | Gateway, cron, sessions |
| `linear` (MCP or API) | Backlog |

When unsure, run `<tool> --help` first.
