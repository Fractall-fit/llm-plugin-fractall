---
name: fractall-fit
description: Query Fractall teams and athlete rosters. Use when the user asks about their club, teams, squads, or athletes in Fractall.
---

# Fractall.fit

## Overview

This skill connects Codex to the hosted Fractall MCP server. Use it to verify the connection, list teams visible to the signed-in user, fetch a team by ID, and load athlete rosters for a team.

## Prerequisites

- The Fractall.fit plugin must be installed and authenticated in Codex.
- The user must have an active Fractall account linked to their OAuth identity.
- Team and roster tools are intended for admin, super_admin, or coach roles.

## Available tools

- `get_connection_status` — verify MCP connectivity and show the resolved Fractall auth context.
- `get_teams` — list teams visible to the signed-in user.
- `get_team_by_id` — fetch one team when you already have a team UUID.
- `get_team_athletes` — fetch athletes and profile fields for a team UUID.

## Required workflow

**Follow these steps in order when answering Fractall questions.**

### Step 0: Connect Fractall (if tools are unavailable)

If Fractall tools are missing, stop and ask the user to:

1. Install the Fractall.fit plugin in Codex.
2. Complete the browser login when Codex prompts for OAuth.
3. Retry the request in a new message after authentication succeeds.

### Step 1: Confirm the goal

Clarify whether the user wants:

- a connection check,
- a team list,
- one team’s details,
- or a roster for a specific team.

If they name a team by label instead of ID, call `get_teams` first and match by name.

### Step 2: Read before summarizing

Use tools in this order:

1. `get_connection_status` when auth or connectivity is uncertain.
2. `get_teams` to discover accessible teams.
3. `get_team_by_id` only when a UUID is known or selected from `get_teams`.
4. `get_team_athletes` after you have the correct `teamId`.

### Step 3: Present results clearly

- Summarize teams with names and IDs when follow-up actions may be needed.
- For rosters, group athletes logically and call out missing or sparse profile fields.
- If a tool returns an error, explain it in plain language and suggest the next step.

## Example prompts

- "List my Fractall teams"
- "Who is on the U18 squad?"
- "Show roster details for team `<uuid>`"
- "Check whether Fractall MCP is connected"
