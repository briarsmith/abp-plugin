# ABP Planning Plugin Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a Claude Code plugin that bundles ABP's MCP server with a planning skill so agents think before they browse.

**Architecture:** Standalone plugin repo with 4 files — plugin manifest, MCP config, planning skill, and README. No build step, no dependencies. The `.mcp.json` launches ABP via npx, the skill provides planning guidance.

**Tech Stack:** Claude Code plugin format (JSON + Markdown)

---

### Task 1: Plugin Manifest

**Files:**
- Create: `.claude-plugin/plugin.json`

**Step 1: Create the plugin manifest**

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

**Step 2: Validate JSON is well-formed**

Run: `cat .claude-plugin/plugin.json | python3 -m json.tool`
Expected: Pretty-printed JSON output, no errors.

**Step 3: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: add plugin manifest"
```

---

### Task 2: MCP Server Configuration

**Files:**
- Create: `.mcp.json`

**Step 1: Create the MCP config**

```json
{
  "browser": {
    "type": "stdio",
    "command": ["npx", "-y", "agent-browser-protocol", "--mcp"]
  }
}
```

**Step 2: Validate JSON is well-formed**

Run: `cat .mcp.json | python3 -m json.tool`
Expected: Pretty-printed JSON output, no errors.

**Step 3: Commit**

```bash
git add .mcp.json
git commit -m "feat: add MCP server config for ABP"
```

---

### Task 3: Planning Skill

This is the core of the plugin. The skill triggers when the agent is about to browse and guides it through a planning step.

**Files:**
- Create: `skills/browse-planning/SKILL.md`

**Step 1: Create the skill file**

```markdown
---
name: browse-planning
description: "Use before any web browsing task — searching, finding, booking, ordering, navigating, or any task involving website interaction. Triggers on requests like 'find', 'search', 'book', 'order', 'buy', 'look up', 'go to', 'check', or 'compare' that require using browser tools."
---

# Browse Planning

## Overview

Before using any browser tool, pause and think about the task. Do not start clicking until you have a plan.

## Before Touching the Browser

### 1. Restate the Goal

Summarize what the user wants in one sentence. Be specific.

### 2. Identify Ambiguities

Check for underspecified parameters. Common ambiguities:

- **Dates:** specific dates or flexible? Date range or single date?
- **Direction:** one-way or round trip?
- **Preferences:** cheapest, fastest, highest-rated, most convenient?
- **Quantity/scope:** how many results? All options or just the best?
- **Location specifics:** exact address, neighborhood, city, or region?
- **Account/login:** does this require being logged in?

### 3. Decide: Clarify or Proceed

- **Clear task** (e.g., "go to google.com", "open twitter") → state your brief plan and proceed immediately. Do not ask unnecessary questions.
- **Ambiguous task** (e.g., "find the cheapest flight to Bali in April") → ask the user the 1-2 most important clarifying questions before opening the browser.
- **Rule of thumb:** if a wrong assumption would waste 5 or more browser actions, ask first. If the task is straightforward, just go.

### 4. State the Plan

Before your first browser action, briefly announce:
- Which site you'll use and why
- What steps you'll take
- What assumptions you're making (if any)

Example: "I'll go to Google Flights, search for one-way flights from SFO to Bali with flexible dates in April, sorted by cheapest. Proceeding now."

## During Execution

### Re-plan on Failure

If your last 2 browser actions didn't make progress toward the goal, STOP. Do not retry the same actions. Instead:
1. Take a screenshot and assess where you are
2. Identify what went wrong
3. Form a new approach
4. Announce the revised plan before continuing

### Handle Unexpected States

- **Popup or dialog:** dismiss it or handle it, then continue
- **Login wall:** ask the user if they want to log in or try a different approach
- **Wrong page:** navigate back and try a different path
- **Error or dead end:** tell the user what happened and suggest alternatives

### Know When to Stop

If the task seems impossible (site is down, information doesn't exist, login required), tell the user clearly:
- What you tried
- Why it didn't work
- What alternatives exist
```

**Step 2: Verify the frontmatter parses correctly**

Run: `head -4 skills/browse-planning/SKILL.md`
Expected: YAML frontmatter with `name: browse-planning` and a description starting with "Use before any web browsing task".

**Step 3: Commit**

```bash
git add skills/browse-planning/SKILL.md
git commit -m "feat: add browse-planning skill"
```

---

### Task 4: README

**Files:**
- Create: `README.md`

**Step 1: Create the README**

```markdown
# ABP Plugin

A Claude Code plugin for [Agent Browser Protocol](https://github.com/theredsix/agent-browser-protocol) that adds smart task planning to browser automation.

## What This Does

When you install this plugin, you get:
- **ABP's browser tools** — click, type, navigate, screenshot, and more via MCP
- **A planning skill** — guides Claude to think before browsing: identify ambiguities, ask clarifying questions when needed, and form a strategy before clicking

## Install

```bash
/plugin add /path/to/abp-plugin
```

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
```

**Step 2: Commit**

```bash
git add README.md
git commit -m "feat: add README"
```

---

### Task 5: Test the Plugin Locally

**Step 1: Install the plugin in Claude Code**

Run: `/plugin add /Users/briarsmith/Documents/Coding/abp-plugin`

**Step 2: Verify the MCP server starts**

Start a new Claude Code session and check that browser tools are available.

**Step 3: Test the planning skill triggers**

Ask Claude: "Find me the cheapest flight from SFO to Bali in April"

Expected: Claude should restate the goal, identify ambiguities (one-way vs round trip, specific dates vs flexible), and ask clarifying questions before opening the browser.

**Step 4: Test a clear task doesn't over-plan**

Ask Claude: "Go to news.ycombinator.com"

Expected: Claude should state a brief plan and proceed immediately without asking unnecessary questions.
