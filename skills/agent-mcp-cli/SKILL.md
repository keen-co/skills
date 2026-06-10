---
name: agent-mcp-cli
description: >-
  Configure MCP and use shell CLIs across agent runtimes. Use when setting up
  MCP servers, choosing between MCP vs skills vs shell CLI, detecting which agent
  runtime is active, or emitting MCP config for Claude Code, Cursor, Hermes,
  OpenClaw, or Multica. Requires @keen/cli — run keen --help first.
---

# Agent MCP + CLI

## Install the CLI first

Skills teach workflow; **`@keen/cli`** owns the runtime matrix, MCP config, and recipes.

```bash
npm install -g @keen/cli
# or
npx @keen/cli --help
```

If this skill is missing from your agent:

```bash
npx skills add keen-co/skills --skill agent-mcp-cli -y
```

## Always use CLI self-docs before guessing

The CLI is self-documenting. Prefer these commands over memorized syntax:

```bash
keen --help
keen runtime --help
keen mcp --help
keen runtime detect
keen runtime matrix --json
keen runtime show claude-code
keen mcp recipe list
keen mcp recipe show linear
keen mcp emit --runtime claude-code --recipe linear
keen mcp emit --runtime hermes --recipe linear
keen mcp validate --runtime cursor
```

## MCP vs Skill vs Shell CLI

| Need | Use |
| ---- | --- |
| Teach workflow / conventions | **Skill** (this file) |
| Runtime matrix, MCP config, recipes | **`keen` CLI** |
| Call external system with schema'd tools | **MCP server** |
| One-off terminal operation | **Shell CLI** (`gh`, `vercel`, `wrangler`, …) |

## Critical: Multica MCP pass-through

Only **Claude Code** consumes Multica `agent.mcp_config`. Other runtimes store or ignore it silently.

Before setting MCP via Multica for a non-Claude runtime:

```bash
keen runtime show <runtime-id>
keen mcp validate --runtime <runtime-id>
```

Configure MCP locally for Cursor, Hermes, OpenClaw, etc.

## Common workflows

### Detect runtimes on this machine

```bash
keen runtime detect
```

### Emit MCP config for Claude Code

```bash
keen mcp emit --runtime claude-code --recipe linear
```

Merge into `.mcp.json` or pass via `--mcp-config`.

### Emit MCP config for Hermes

```bash
keen mcp emit --runtime hermes --recipe linear
```

Merge into `~/.hermes/config.yaml` under `mcp_servers`.

### List portfolio MCP recipes

```bash
keen mcp recipe list
keen mcp recipe show github
keen mcp recipe show sentry
keen mcp recipe show hermes
keen mcp recipe show startupkit
```

## Security

Run before adding MCP servers:

```bash
keen mcp validate --runtime <runtime-id>
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
