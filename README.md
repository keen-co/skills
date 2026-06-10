[![skills.sh](https://skills.sh/b/jot-so/skills)](https://skills.sh/jot-so/skills)

# Jot Skills

Agent skills for working with Jot tooling and 01 portfolio operations.

Install via the [skills CLI](https://github.com/vercel-labs/skills).

```bash
# Install all skills
npx skills add jot-so/skills -y

# Install a specific skill
npx skills add jot-so/skills --skill agent-mcp-cli -y
```

## Prerequisites

Most skills assume **`@jot/cli`** is installed:

```bash
npm install -g @jot/cli
jot --help
```

## Available Skills

| Skill | Description |
| ----- | ----------- |
| `agent-mcp-cli` | Configure MCP and use shell CLIs across agent runtimes (Claude Code, Cursor, Hermes, OpenClaw, …). Delegates to `@jot/cli`. |
