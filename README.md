# ABP Plugin

A Claude Code plugin for [Agent Browser Protocol](https://github.com/theredsix/agent-browser-protocol) that adds smart task planning to browser automation.

## What This Does

When you install this plugin, you get:
- **ABP's browser tools** — click, type, navigate, screenshot, and more via MCP
- **A planning skill** — guides Claude to think before browsing: identify ambiguities, ask clarifying questions when needed, and form a strategy before clicking

## Install

### Local

```bash
claude --plugin-dir /path/to/abp-plugin
```

### Marketplace

Coming soon.

## How It Works

Without the plugin, agents using ABP tend to dive straight into browser actions without thinking. For example, "find the cheapest flight to Bali in April" might become a round-trip search for April 1-30 — the agent never paused to consider one-way vs round trip or flexible dates.

With the plugin, the agent:
1. Restates the goal
2. Identifies what's ambiguous
3. Asks you to clarify (if needed) or states its assumptions
4. Announces its plan
5. Then starts browsing

For clear tasks ("go to google.com"), it just goes — no unnecessary questions.

## Plugin Structure

- `.claude-plugin/plugin.json` — plugin metadata
- `.mcp.json` — launches ABP's MCP server
- `skills/browse-planning/SKILL.md` — the planning skill
