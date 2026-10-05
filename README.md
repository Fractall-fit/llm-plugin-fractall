# Fractall.fit AI App Plugins

Connect Codex or Claude Code to the hosted Fractall MCP server at
`https://mcp.fractall.fit/mcp`.

This repo packages the same integration for both apps. Tool implementations live
on the hosted server. The plugin ships HTTP MCP plus OAuth config and the coach
skill.

Install, sign-in, and update steps are in
[`plugins/fractall-fit/README.md`](plugins/fractall-fit/README.md).

## Codex

```sh
codex plugin marketplace add Fractall-fit/llm-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

Codex prompts for Fractall OAuth on install. After updates:

```sh
codex plugin update fractall-fit@fractall-fit
```

## Claude Code

```text
/plugin marketplace add Fractall-fit/llm-plugin-fractall
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

Authenticate the `fractall-fit` MCP server from `/mcp` if prompted. After
updates:

```text
/plugin marketplace update fractall-fit
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

## Tools on the hosted server

Live discovery can add tools without a plugin release. The coach skill in this
repo should match the current server surface:

**Orient:** `get_connection_status`, `get_teams`, `get_team_by_id`,
`get_team_athletes`, `get_athlete_profile`

**Calendar:** `get_team_calendar`, `get_activity`, `list_activities`

**Wellness:** `get_team_wellness`, `get_wellness_non_responders`,
`get_athlete_wellness`, `list_wellness_check_ins`

**Load / RPE:** `get_team_load`, `get_team_rpe`, `get_athlete_load`,
`get_rpe_non_responders`, `list_rpe_submissions`

**Injuries:** `get_team_injuries`, `get_injury`, `get_pre_injury_context`,
`list_injury_cases`

**GPS:** `get_gps_activities`, `get_team_gps`, `get_athlete_gps`,
`get_activity_gps`, `get_gps_microcycle`, `list_gps_session_metrics`,
`get_acwr`

**Physical tests:** `get_team_physical_tests`, `get_physical_tests_on_date`,
`get_athlete_physical_tests`, `list_physical_test_measurements`

**Charts / HTML reports:** `search_visualization_primitives`,
`get_visualization_primitive`

RPE session-load ACWR comes from `get_team_load` / `get_athlete_load` with
`includeMetrics=true`. GPS volume ACWR (distance, HSR, sprint, player load,
duration, accel/decel) comes from `get_acwr`.
