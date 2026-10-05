# Fractall.fit for AI Apps

Connect Codex, Claude Code, or Cursor to your Fractall club and ask about
teams, the calendar, wellness, RPE and GPS load, injuries, physical tests, and
chart-style reports in plain language.

## Before You Start

- Install [Codex](https://chatgpt.com/codex) or [Claude Code](https://code.claude.com).
- Use the same email you use for Fractall.
- Your Fractall account must already exist and be active.

## Install In Codex

Run these two commands in Terminal:

```sh
codex plugin marketplace add Fractall-fit/llm-plugin-fractall
codex plugin add fractall-fit@fractall-fit
```

## Install In Claude Code

In Claude Code, run:

```text
/plugin marketplace add Fractall-fit/llm-plugin-fractall
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

## Sign In With Codex

Codex opens your browser during install. Log in with Fractall and approve access.

If login fails or tools do not appear:

1. Quit and reopen Codex.
2. Run `codex plugin add fractall-fit@fractall-fit` again.
3. Retry your question in a new chat.

## Sign In With Claude Code

Claude Code may ask you to authenticate the MCP server the first time it loads the plugin or calls a Fractall tool.

If login does not start automatically:

1. Run `/mcp` inside Claude Code.
2. Select the `fractall-fit` server.
3. Follow the browser login flow.
4. Run `/reload-plugins` if the tools still do not appear.

## Try It

Ask things like:

- "List my Fractall teams"
- "Who hasn't checked in wellness today?"
- "Show squad RPE load with ACWR highlights"
- "Show GPS ACWR for distance and high-speed running"
- "Build a squad load chart I can read in this chat"
- "Who is currently injured or in RTP?"

## Update Later

Codex:

```sh
codex plugin update fractall-fit@fractall-fit
```

Claude Code:

```text
/plugin marketplace update fractall-fit
/plugin install fractall-fit@fractall-fit
/reload-plugins
```

## Need Help?

Contact your Fractall admin if OAuth works but no teams appear. That usually means your account is not linked to a club yet.
