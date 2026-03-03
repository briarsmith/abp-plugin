# ABP Planning Plugin Design

## Problem

When LLM agents use ABP to browse the web, they act reactively — diving into browser actions without thinking about the task first. This leads to wrong assumptions (e.g., "cheapest flight in April" becomes round-trip April 1-30 instead of one-way, flexible dates) and wasted steps.

The core issue: the agent doesn't pause to analyze ambiguity, clarify requirements, or form a strategy before clicking.

## Solution

A Claude Code plugin that bundles ABP's MCP server with a **planning skill**. The skill triggers before browsing tasks and guides the agent to think before it acts.

## Plugin Structure

```
abp-plugin/
├── .claude-plugin/
│   └── plugin.json                  # Metadata (name, author, description)
├── .mcp.json                        # Launches ABP MCP server via npx
├── skills/
│   └── browse-planning/
│       └── SKILL.md                 # The planning skill
└── README.md
```

### `.claude-plugin/plugin.json`

```json
{
  "name": "agent-browser-protocol",
  "description": "Browser automation for AI agents with smart task planning",
  "author": {
    "name": "Han Wang"
  },
  "homepage": "https://github.com/theredsix/agent-browser-protocol",
  "repository": "https://github.com/theredsix/abp-plugin",
  "license": "BSD-3-Clause"
}
```

### `.mcp.json`

```json
{
  "browser": {
    "type": "stdio",
    "command": ["npx", "-y", "agent-browser-protocol", "--mcp"]
  }
}
```

## Planning Skill Design

### Trigger

The skill activates when the agent is about to perform any web browsing task — search, find, book, navigate, order, look up, or any task involving website interaction.

### Flow

**Before touching the browser:**

1. **Restate the goal** — summarize what the user wants in one sentence.

2. **Identify ambiguities** — check for underspecified parameters. Common ambiguities include:
   - Dates (specific vs flexible)
   - Direction (one-way vs round trip)
   - Preferences (cheapest vs fastest vs most convenient)
   - Quantity, size, or scope
   - Location or destination specifics

3. **Branch on ambiguity:**
   - **Clear task** (e.g., "go to google.com") → state a brief plan, proceed immediately.
   - **Ambiguous task** (e.g., "find cheapest flight") → ask the user the 1-2 most important clarifying questions before browsing.
   - **Rule of thumb:** if a wrong assumption would waste 5+ browser actions, ask first.

4. **State the plan** — before the first browser action, announce the approach: which site, what steps, what assumptions.

**During execution:**

5. **Re-plan on failure** — if the last 2 actions didn't make progress toward the goal, stop and re-evaluate the approach instead of retrying blindly.

6. **Handle unexpected states** — popups, login walls, errors, dead ends. Handle or ask the user rather than flailing.

7. **Know when to stop** — if the task seems impossible, tell the user why and suggest alternatives.

## Distribution

- **Claude Code users:** install the plugin (one command). Gets ABP MCP server + planning skill automatically.
- **Non-Claude Code users:** continue using `npx agent-browser-protocol --mcp` directly (unchanged).

## Approach Rationale

- **Plugin over pure MCP:** MCP tool descriptions are too small for meaningful planning guidance. A skill can provide structured reasoning instructions.
- **Adaptive over rigid:** The skill scales with ambiguity. Clear tasks proceed fast; ambiguous tasks get clarification. This avoids the overhead of forcing every task through a heavy planning phase.
- **Standalone repo:** Keeps the planning skill separate from ABP's C++ source and npm package. Easy to iterate on the skill without touching the browser engine.

## Out of Scope

- ABP-side changes (no new tools or API endpoints)
- Support for non-Claude Code clients (they use the bare MCP server)
- Structured plan persistence or plan approval workflows
