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
