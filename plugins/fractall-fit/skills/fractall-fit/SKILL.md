---
name: fractall-fit
description: >-
  Query Fractall coach data — teams, athletes, wellness, training load/RPE, and
  injuries — via the hosted Fractall MCP server. Use when the user asks about
  their club, squads, check-ins, load/ACWR, who is injured, or pre-injury context.
---

# Fractall.fit

## Overview

This skill connects the AI app to the hosted Fractall MCP server at
`https://mcp.fractall.fit/mcp`. Tools return **Coach Data Lens** envelopes
(summary-first JSON with names and KPIs). Reply in plain language — never dump
raw JSON or UUIDs to the user. Use IDs only for follow-up tool calls.

## Prerequisites

- Fractall.fit plugin installed and authenticated (OAuth) in the current AI app.
- Active Fractall account linked to the OAuth identity.
- Tools are intended for `admin`, `super_admin`, or `coach` roles.

If tools are missing or MCP status shows an error: ask the user to open MCP
settings, run `mcp_auth` / re-authenticate Fractall.fit, then retry in a new message.

## Tool surface

### Orient

- `get_connection_status` — MCP + auth context
- `get_teams` — teams visible to the signed-in user
- `get_team_by_id` — one team by UUID
- `get_team_athletes` — roster summary (optional full profiles)
- `get_athlete_profile` — one athlete profile
- `get_recent_sessions` — recent team sessions (default last 7 days)

### Wellness

- `get_team_wellness` — squad wellness summary for a date range
- `get_wellness_non_responders` — who has not checked in (defaults to today UTC)
- `get_athlete_wellness` — one athlete summary + recent check-ins

### Training load / RPE

- `get_team_load` — per-athlete load table; set `includeMetrics=true` for ACWR / EWMA ACWR / monotony / strain
- `get_team_rpe` — team RPE by session; optional drill-down
- `get_athlete_load` — one athlete load + metrics at end date
- `get_rpe_non_responders` — who missed RPE for a session

### Injuries

- `get_team_injuries` — open cases by default; overview + notable + capped list
- `get_injury` — one case + progress + athlete history
- `get_pre_injury_context` — 7 days before injury (wellness/load) + ACWR/EWMA at injury date

## Required workflow

### Step 0: Auth

If Fractall tools are unavailable or discovery failed, stop and ask the user to
authenticate the Fractall.fit MCP server, then retry.

### Step 1: Orient

1. Prefer `get_connection_status` when auth/connectivity is uncertain.
2. Call `get_teams` to discover accessible teams (match by name when the user
   does not provide a UUID).
3. Use `get_team_by_id` / `get_team_athletes` only after you have the right `teamId`.

### Step 2: Pick the right domain tool

| Coach question | Tool |
| --- | --- |
| How is squad wellness? | `get_team_wellness` |
| Who hasn't checked in? | `get_wellness_non_responders` |
| Squad load / ACWR / strain | `get_team_load` with `includeMetrics=true` |
| Who missed RPE? | `get_rpe_non_responders` |
| Who is out / RTP / cleared? | `get_team_injuries` |
| One injury case / progress | `get_injury` |
| How did they look before injury? | `get_pre_injury_context` |

Do **not** answer squad load questions with repeated `get_athlete_load` calls.
Do **not** call `get_injury` before `get_team_injuries` for squad injury questions.

### Step 3: Present results

- Lead with highlights and notable athletes, then supporting numbers.
- Use athlete/team **names** and dates — never UUIDs in user-visible text.
- If a tool errors, explain plainly and suggest the next step.

## Example prompts

- "List my Fractall teams"
- "Who hasn't submitted wellness today?"
- "Show squad load and who is above ACWR 1.3"
- "Who is currently injured or in RTP?"
- "What did wellness and load look like before that hamstring injury?"
