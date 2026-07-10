# Fractall.fit Codex Plugin

Codex plugin marketplace package for the hosted Fractall MCP server at `https://mcp.fractall.fit/mcp`.

## Layout

```text
packages/codex-plugin-fractall/
├── .agents/plugins/marketplace.json
└── plugins/fractall-fit/
    ├── .codex-plugin/plugin.json
    ├── .mcp.json
    ├── assets/
    ├── skills/fractall-fit/SKILL.md
    └── README.md
```

## Local test

From a machine with Codex installed:

```sh
codex plugin marketplace add /absolute/path/to/packages/codex-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

Codex should prompt for OAuth against Fractall on install. Then ask:

- "Check Fractall connection status"
- "List my teams"

## Share with non-technical users

1. Push this folder to its own repo, for example `Fractall-fit/codex-plugin-fractall`.
2. Send founders the install steps in `plugins/fractall-fit/README.md`.
3. Bump `plugins/fractall-fit/.codex-plugin/plugin.json` `version` when MCP URLs or copy change.
4. Tell users to run `codex plugin update fractall-fit@fractall-fit` after updates.

## MCP configuration

The plugin bundles HTTP MCP with OAuth resource metadata:

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

Production MCP must keep:

- `MCP_AUTH_MODE=oauth`
- `MCP_OAUTH_REQUIRED_SCOPES` unset
- Supabase OAuth 2.1 + dynamic client registration enabled

## Tools exposed

- `get_connection_status`
- `get_teams`
- `get_team_by_id`
- `get_team_athletes`

See `apps/mcp-server/README.md` for server operations and troubleshooting.
