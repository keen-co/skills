# Keen Skills

Agent skills for working with Keen tooling and 01 portfolio operations.

Install via the [skills CLI](https://github.com/vercel-labs/skills).

```bash
# Install all skills
npx skills add keen-co/skills -y

# Install a specific skill
npx skills add keen-co/skills --skill agent-mcp-cli -y
```

## Prerequisites

Most skills assume **`@keen/cli`** is installed:

```bash
npm install -g @keen/cli
keen --help
```

## Available Skills

| Skill | Description |
| ----- | ----------- |
| `agent-mcp-cli` | Configure MCP and use shell CLIs across agent runtimes (Claude Code, Cursor, Hermes, OpenClaw, …). Delegates to `@keen/cli`. |
