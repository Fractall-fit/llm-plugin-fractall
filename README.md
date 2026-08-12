# Fractall.fit AI App Plugins

Plugin packages for connecting AI apps to the hosted Fractall MCP server at `https://mcp.fractall.fit/mcp`.

Currently this repo packages the same Fractall integration for:

- Codex, via `.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json`
- Claude Code, via `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`

## Layout

```text
llm-plugin-fractall/
├── .agents/plugins/marketplace.json
├── .claude-plugin/marketplace.json
└── plugins/fractall-fit/
    ├── .claude-plugin/plugin.json
    ├── .codex-plugin/plugin.json
    ├── .mcp.json
    ├── .mcp.claude.json
    ├── assets/
    ├── skills/fractall-fit/SKILL.md
    └── README.md
```

## Codex Local Test

From a machine with Codex installed:

```sh
codex plugin marketplace add /absolute/path/to/llm-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

Codex should prompt for OAuth against Fractall on install. Then ask:

- "Check Fractall connection status"
- "List my teams"

## Claude Code Local Test

From this repo root:

```sh
claude --plugin-dir ./plugins/fractall-fit
```

Inside Claude Code, run:

```text
/fractall-fit:fractall-fit
```

For OAuth, run `/mcp` inside Claude Code and authenticate the `fractall-fit` server if prompted.

## Release Checklist

1. Publish updates to `Fractall-fit/llm-plugin-fractall`.
2. Send users the install steps in `plugins/fractall-fit/README.md`.
3. Bump plugin manifest versions when MCP URLs, skill, or copy change:
   - `plugins/fractall-fit/.codex-plugin/plugin.json`
   - `plugins/fractall-fit/.claude-plugin/plugin.json`
   - `.claude-plugin/marketplace.json` (plugin version field)
4. Tell Codex users to run `codex plugin update fractall-fit@fractall-fit` after updates.
5. Tell Claude Code users to run `/plugin marketplace update fractall-fit`, then `/plugin install fractall-fit@fractall-fit` if needed.

## MCP Configuration

The Codex plugin bundles HTTP MCP with OAuth resource metadata:

```json
{
  "mcpServers": {
    "fractall-fit": {
      "type": "http",
      "url": "https://mcp.fractall.fit/mcp",
      "oauth_resource": "https://mcp.fractall.fit"
    }
  }
}
```

The Claude Code plugin uses Claude's documented MCP config shape and relies on standard OAuth discovery:

```json
{
  "mcpServers": {
    "fractall-fit": {
      "type": "http",
      "url": "https://mcp.fractall.fit/mcp",
      "oauth": {
        "scopes": "openid email profile"
      }
    }
  }
}
```

## Maintainer MCP Notes

The production MCP server must keep:

- `MCP_AUTH_MODE=oauth`
- `MCP_OAUTH_REQUIRED_SCOPES` unset
- Supabase OAuth 2.1 + dynamic client registration enabled

## Tools Exposed

Tool implementations live on the hosted MCP server. This plugin only packages
HTTP MCP + OAuth config and the coach skill. Current server surface includes:

**Orient:** `get_connection_status`, `get_teams`, `get_team_by_id`,
`get_team_athletes`, `get_athlete_profile`, `get_recent_sessions`

**Wellness:** `get_team_wellness`, `get_wellness_non_responders`,
`get_athlete_wellness`

**Load / RPE:** `get_team_load`, `get_team_rpe`, `get_athlete_load`,
`get_rpe_non_responders`

**Injuries:** `get_team_injuries`, `get_injury`, `get_pre_injury_context`

If an AI app still shows only the original four tools, live MCP discovery
failed (usually OAuth). Re-authenticate the Fractall.fit MCP server, then
update/reload the plugin so the skill and tool list refresh.
