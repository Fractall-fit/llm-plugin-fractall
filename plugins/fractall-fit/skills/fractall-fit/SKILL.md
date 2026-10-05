---
name: fractall-fit
description: >-
  Query Fractall coach data — teams, athletes, calendar, wellness, RPE load,
  GPS volume/ACWR, injuries, physical tests, and HTML report charts — via the
  hosted Fractall MCP server. Use when the user asks about their club, squads,
  schedule, check-ins, RPE or GPS ACWR, GPS load, who is injured, tests, or a
  chart-style report in chat.
---

# Fractall.fit

## Overview

This skill connects the AI app to the hosted Fractall MCP server at
`https://mcp.fractall.fit/mcp`. Tools return **Coach Data Lens** envelopes
(summary-first JSON with names and KPIs). Reply in plain language. Never dump
raw JSON or UUIDs to the user. Use IDs only for follow-up tool calls.

For a chart or print-style HTML report, return HTML for the host to render in
chat. Fractall does not paint a report page or share a URL.

## Prerequisites

- Fractall.fit plugin installed and authenticated (OAuth) in the current AI app.
- Active Fractall account linked to the OAuth identity.
- Tools are intended for `admin`, `super_admin`, or `coach` roles.

If tools are missing or MCP status shows an error, ask the user to open MCP
settings, run `mcp_auth` / re-authenticate Fractall.fit, then retry in a new
message.

## Tool surface

### Orient

- `get_connection_status` — MCP + auth context
- `get_teams` — teams visible to the signed-in user
- `get_team_by_id` — one team by UUID
- `get_team_athletes` — roster summary (optional full profiles)
- `get_athlete_profile` — one athlete profile

### Calendar / schedule

- `get_team_calendar` — activities by day, kinds, cycles, breaks, MD labels (last 7 days default)
- `get_activity` — one activity plus that day's calendar context
- `list_activities` — paged activity rows for a custom report

### Wellness

- `get_team_wellness` — squad wellness summary for a date range
- `get_wellness_non_responders` — who has not checked in (defaults to today UTC)
- `get_athlete_wellness` — one athlete summary + recent check-ins
- `list_wellness_check_ins` — paged check-in rows for a custom report

### Training load / RPE

- `get_team_load` — per-athlete load table; set `includeMetrics=true` for RPE session-load ACWR / EWMA ACWR / monotony / strain
- `get_team_rpe` — team RPE by activity; optional drill-down
- `get_athlete_load` — one athlete load + RPE metrics at end date
- `get_rpe_non_responders` — who missed RPE for an activity
- `list_rpe_submissions` — paged RPE/load rows for a custom report

### Injuries

- `get_team_injuries` — open cases by default; overview + notable + capped list
- `get_injury` — one case + progress + athlete history
- `get_pre_injury_context` — 7 days before injury (wellness and RPE load) plus RPE ACWR/EWMA at injury date. Use `get_acwr` if you also need GPS volume ACWR.
- `list_injury_cases` — paged injury-case rows for a custom report

### GPS / external load

- `get_gps_activities` — GPS sessions attached to activities in a range (no metric values)
- `get_team_gps` — squad external load: overview, by session, by athlete, notable data gaps
- `get_athlete_gps` — one athlete GPS totals, peaks, recent GPS sessions
- `get_activity_gps` — GPS session on one activity vs full-history benchmarks
- `get_gps_microcycle` — accumulated microcycle volume vs previous microcycles
- `list_gps_session_metrics` — paged GPS metric rows for a custom report
- `get_acwr` — rolling and EWMA ACWR on GPS volume: distance, HSR, sprint distance, player load, duration, accel/decel efforts

### Physical tests

- `get_team_physical_tests` — squad physically right now; latest-in-window category scores /100
- `get_physical_tests_on_date` — batteries on a calendar day, not activities
- `get_athlete_physical_tests` — one athlete category scores plus recent test events
- `list_physical_test_measurements` — paged measurement rows for a custom report

### Visualization catalog

- `search_visualization_primitives` — list or search visual blocks the host renders as HTML in chat
- `get_visualization_primitive` — one primitive's HTML/CSS snippet, data tools, and v1 chart spec

`list_*` tools are for custom reports (paged rows). Prefer the `get_*` summary
tools for ordinary coach questions.

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
| What's on the calendar / this week's sessions? | `get_team_calendar` |
| One activity's details | `get_activity` |
| Squad RPE load / RPE ACWR / strain | `get_team_load` with `includeMetrics=true` |
| One athlete's RPE load / RPE ACWR | `get_athlete_load` |
| GPS volume ACWR (distance, HSR, sprint, player load, duration, accel/decel) | `get_acwr` |
| Squad GPS / external load | `get_team_gps` |
| Who missed RPE? | `get_rpe_non_responders` |
| Who is out / RTP / cleared? | `get_team_injuries` |
| One injury case / progress | `get_injury` |
| How did they look before injury? | `get_pre_injury_context` |
| Physical tests / "are they physically right now?" | `get_team_physical_tests` |
| Chart or HTML report in chat | `search_visualization_primitives`, then `get_visualization_primitive`, then the matching data tools |

Do **not** answer GPS volume ACWR with `get_team_load`. That tool's ACWR is RPE
session-load ACWR.
Do **not** answer RPE ACWR with `get_acwr`. That tool is GPS volume ratios.
Do **not** answer squad load questions with repeated `get_athlete_load` calls.
Do **not** call `get_injury` before `get_team_injuries` for squad injury questions.
Do **not** invent a second chart language. Fill the catalog spec from data tools.
Do **not** rewrite a visualization as an athlete-summary page.

### Step 3: Present results

- Lead with highlights and notable athletes, then supporting numbers.
- Use athlete/team **names** and dates. Never UUIDs in user-visible text.
- For charts, return the catalog HTML for the host to render. Fractall does not
  paint a report page.
- If a tool errors, explain plainly and suggest the next step.

## Example prompts

- "List my Fractall teams"
- "What's on the calendar this week?"
- "Who hasn't submitted wellness today?"
- "Show squad load and who is above RPE ACWR 1.3"
- "Show GPS ACWR for distance and HSR"
- "Who is currently injured or in RTP?"
- "What did wellness and load look like before that hamstring injury?"
- "Build a squad load chart I can read in this chat"
